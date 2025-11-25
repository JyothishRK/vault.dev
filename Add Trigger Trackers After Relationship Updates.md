

  

## Problem Statement

  

When `updateMany` operations modify role/group/member documents (e.g., adding members to roles, adding roles to groups), these changes are not captured by trigger trackers. The trigger processors need these trackers to update member `license_type` and `read_only_flag` correctly.

  

## Solution Approach

  

Call the appropriate trigger tracker functions after `updateMany` operations. The functions have built-in early exit logic that checks if relevant fields changed, so they will:

  

- Exit quickly if no relevant fields changed (minimal performance impact)

- Create trackers only when needed (relevant fields changed)

  

## Implementation Locations

  

### 1. Member Create/Update Operations

  

#### File: `organisation/users.js`

  

**Location 1: `memberCreate` (line ~5965)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $push: { member_ids: data.member_id } })`

- **Action**: Create **member trigger tracker** for the newly created member (already exists at line 6018)

- **Note**: The member tracker already exists, but we should also create member trackers for any existing members whose roles might have been affected indirectly

  

**Location 2: `memberCreate` (line ~5971)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: data.usergroup_id } }, { $push: { member_ids, associatedUsers } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

- **Implementation**: After updateMany, fetch updated group documents and call `createUsergroupTriggerTracker` for each

  

**Location 3: `subscriptionPendingActivate` (line ~10049)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $push: { member_ids: member_insert.member_id } })`

- **Action**: Create **member trigger tracker** for the activated member (already exists at line 10009)

- **Note**: Similar to memberCreate - member tracker exists, but consider if role trackers needed

  

**Location 4: `subscriptionPendingActivate` (line ~10055)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: member_insert.usergroup_id } }, { $push: { member_ids, associatedUsers } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

  

### 2. Role Operations

  

#### File: `organisation/roles.js`

  

**Location 5: `saveRoleInfo` (line ~133)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: response.member_ids } }, { $push: { roles } })`

- **Action**: Create **member trigger trackers** for all affected members

- **Implementation**: After updateMany, fetch updated member documents and call `createMemberTriggerTracker` for each

  

**Location 6: `saveRoleInfo` (line ~137)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: response.group_id } }, { $push: { roles } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

- **Implementation**: After updateMany, fetch updated group documents and call `createUsergroupTriggerTracker` for each

  

**Location 7: `updateRoleInfo` - licenseType update (line ~816-817)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: mem_ids } }, { $pull/push: { roles } })`

- **Action**: Create **member trigger trackers** for affected members

- **Note**: This happens when licenseType changes, members need license_type recalculated

  

**Location 8: `updateRoleInfo` - users.newUsers (line ~825)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: get_body.users.newUsers } }, { $push: { roles } })`

- **Action**: Create **member trigger trackers** for newly added members

  

**Location 9: `updateRoleInfo` - users.removedUsers (line ~831)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: get_body.users.removedUsers } }, { $pull: { roles } })`

- **Action**: Create **member trigger trackers** for removed members

  

**Location 10: `updateRoleInfo` - groups.newGroups (line ~844)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: get_body.groups.newGroups } }, { $push: { roles } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

  

**Location 11: `updateRoleInfo` - groups.removedGroups (line ~852)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: get_body.groups.removedGroups } }, { $pull: { roles } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

  

### 3. Group Operations

  

#### File: `organisation/groups.js`

  

**Location 12: `newGroup` (line ~238)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: result.member_ids } }, { $push: { usergroup_id } })`

- **Action**: Create **member trigger trackers** for affected members

  

**Location 13: `newGroup` (line ~245)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $push: { group_id: result.group_id } })`

- **Action**: No role tracker needed (role trigger only processes roleActions/licenseType)

- **Note**: This updates role.group_id, but role trigger doesn't process this field

  

**Location 14: `updateGroupItem` - users.newUsers (line ~1077)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: get_body.users.newUsers } }, { $push: { usergroup_id } })`

- **Action**: Create **member trigger trackers** for newly added members

  

**Location 15: `updateGroupItem` - users.removedUsers (line ~1101)**

  

- **Operation**: `MemberSchema.updateMany({ member_id: { $in: get_body.users.removedUsers } }, { $pull: { usergroup_id } })`

- **Action**: Create **member trigger trackers** for removed members

  

**Location 16: `updateGroupItem` - role_ids.newRoles (line ~1124)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $push: { group_id } })`

- **Action**: No role tracker needed (role trigger doesn't process group_id)

  

**Location 17: `updateGroupItem` - role_ids.removedRoles (line ~1131)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $pull: { group_id } })`

- **Action**: No role tracker needed (role trigger doesn't process group_id)

  

### 4. User Update Operations

  

#### File: `organisation/people.js`

  

**Location 18: `updateUser` - role_ids.newRoles (line ~3210)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $push: { member_ids } })`

- **Action**: No role tracker needed (role trigger doesn't process member_ids)

- **Note**: Member tracker already created at line 3186, which will handle the member's license_type update

  

**Location 19: `updateUser` - role_ids.removedRoles (line ~3218)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roles_ids } }, { $pull: { member_ids } })`

- **Action**: No role tracker needed (role trigger doesn't process member_ids)

- **Note**: Member tracker already created, will handle the update

  

**Location 20: `updateUser` - groups operations (if exists)**

  

- **Operation**: Similar pattern for group updates

- **Action**: Create appropriate trackers based on what's updated

  

### 5. Member Activate/Deactivate

  

#### File: `organisation/users.js`

  

**Location 21: `acitvateDeactivate` - group updates (line ~8464-8465)**

  

- **Operation**: `userGroupSchema.updateMany({ group_id: { $in: member_group_ids } }, { $push/pull: { member_ids } })`

- **Action**: Create **usergroup trigger trackers** for affected groups

  

**Location 22: `acitvateDeactivate` - role updates (line ~3174)**

  

- **Operation**: `roles.updateMany({ _id: { $in: roleIdArray } }, { $pull: { member_ids } })`

- **Action**: No role tracker needed (role trigger doesn't process member_ids)

  

## Implementation Strategy

  

### Helper Functions to Create

  

1. **`createMemberTriggerTrackersForMembers(memberIds, organisationId)`**

  

- Fetches member documents by member_ids

- Creates trigger trackers for each member

- Handles batch processing if needed

  

2. **`createUsergroupTriggerTrackersForGroups(groupIds, organisationId)`**

  

- Fetches group documents by group_id

- Creates trigger trackers for each group

- Handles batch processing if needed

  

### Implementation Pattern

  

For each `updateMany` operation:

  

1. Execute the `updateMany` operation

2. If operation affects members: Fetch updated member documents and create member trackers

3. If operation affects groups: Fetch updated group documents and create group trackers

4. If operation affects roles: Only create trackers if roleActions/licenseType changed (which they won't in these cases)

  

### Performance Considerations

  

1. **Early Exit**: Tracker functions check relevant fields and exit early if none changed

2. **Batch Fetching**: Fetch all affected documents in one query when possible

3. **Async Processing**: Use `Promise.all()` for parallel tracker creation when multiple documents affected

4. **Error Handling**: Wrap tracker creation in try-catch to prevent API failures if tracker creation fails

  

## Files to Modify

  

1. `organisation/users.js` - memberCreate, subscriptionPendingActivate, acitvateDeactivate

2. `organisation/roles.js` - saveRoleInfo, updateRoleInfo

3. `organisation/groups.js` - newGroup, updateGroupItem

4. `organisation/people.js` - updateUser

5. `organisation/index.js` - Add helper functions if needed

  

## Testing Considerations

  

1. Verify trackers are created only when relevant fields change

2. Verify early exit works correctly (no unnecessary tracker creation)

3. Verify API response times are not significantly impacted

4. Verify trigger processors correctly handle the new trackers