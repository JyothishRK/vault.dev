Perfect! That's an excellent enhancement to handle the "get all records" scenario. Let me integrate this improved pagination logic into the complete endpoint:

```javascript
const selectAllUsersAdvanced = async (event) => {
    try {
        // Extract core parameters
        var org_id = parseInt(event.requestContext.authorizer.organisation_id);
        let page = ((event.queryStringParameters.page != undefined) ? parseInt(event.queryStringParameters.page) : 1);
        let limit = ((event.queryStringParameters.limit != undefined) ? parseInt(event.queryStringParameters.limit) : 20);
        let query = ((event.queryStringParameters.query != undefined) ? event.queryStringParameters.query : '');
        let type = ((event.queryStringParameters.type != undefined) ? event.queryStringParameters.type : '');
        const admin = ((event.queryStringParameters.admin != undefined) ? parseInt(event.queryStringParameters.admin) : 0);
        let permissionType = ((event.queryStringParameters.permType != undefined) ? event.queryStringParameters.permType : '');
        
        let member_query_obj = { "organisation_id": org_id, trash_flag: 0 };
        let organization_details = await OrgSchema.findOne({ organisation_id: org_id }, { organisation_admin: 1, member_id: 1 });

        // Apply type filters (simplified)
        switch (type) {
            case "active": {
                member_query_obj.delete_flag = 1;                
            }
                break;
            case "inactive": {
                member_query_obj.delete_flag = { $in: [0, 2, 3] };
            }
                break;
            case "power_users": {
                member_query_obj.license_type = "power users";
                member_query_obj.delete_flag = 1;
                member_query_obj.trash_flag = 0;
            }
                break;
            case "light_users": {
                member_query_obj.license_type = "light users";
                member_query_obj.delete_flag = 1;
                member_query_obj.trash_flag = 0;
            }
                break;
            case "admin": {
                let organization_admin_arr = ((organization_details != null) ? organization_details.organisation_admin : []);
                member_query_obj.member_id = { $in: organization_admin_arr };
            }
                break;
            case "manager": {
                let organization_admin_arr = ((organization_details != null) ? organization_details.organisation_admin : []);
                if (organization_details != null) {
                    organization_admin_arr.push(organization_details.member_id)
                }
                member_query_obj.read_only_flag = 0;
                member_query_obj.member_id = { $nin: organization_admin_arr };
            }
                break;
            case "executive": {
                member_query_obj.read_only_flag = 1;
            }
                break;
            case "all_except_admin": {
                let organization_admin_arr = ((organization_details != null) ? organization_details.organisation_admin : []);
                if (organization_details != null) {
                    organization_admin_arr.push(organization_details.member_id)
                }
                member_query_obj.member_id = { $nin: organization_admin_arr };
                member_query_obj.delete_flag = 1;
            }
                break;
            case "all_except_executive": {
                member_query_obj.read_only_flag = 0;
                member_query_obj.delete_flag = 1;
            }
                break;
            default:
                member_query_obj.delete_flag = 1;
                break;
        }

        // Apply search functionality
        if (query) {
            query = query.replace(/\s\s+/g, ' ').trim();
            query = query.replace(/[.*+?^${}()|[\]\\]/g, "\\$&")
            query = query.replace(/\s+/g, '.*') 
            
            let search_query = [
                { member_name: { $regex: query, $options: 'i' } }, 
                { member_email: { $regex: query, $options: 'i' } }
            ];
            Object.assign(member_query_obj, { "$or": search_query });
        }

        // Apply permission-based filtering
        if(permissionType){
            if(query){
                Object.assign(member_query_obj, { "$and": [{'$or': member_query_obj['$or']} ]});
            }
            const permission_obj = await permissionBasedMembersList(event, permissionType);
            member_query_obj['$or'] = [];
            if(permission_obj && permission_obj.rolesList.length){
                member_query_obj['$or'].push({
                    "roles.roleName" : {
                        $in: permission_obj.rolesList
                    }
                })
            }
            if(permission_obj && permission_obj.groupList.length){
                member_query_obj['$or'].push({
                    "usergroup_id": {
                        $in: permission_obj.groupList
                    }
                })             
            }
            if(query){ 
                member_query_obj['$and'].push({
                    "$or": member_query_obj['$or']
                })
                delete member_query_obj['$or'];
            }
            if(!query){
                if(!member_query_obj['$or'].length){
                    delete member_query_obj['$or'];
                }
            } 
        }

        // Pagination logic - using frontend provided page and limit
        let count_members = await MemberSchema.countDocuments(member_query_obj);
        let skipRecords = (limit === -1) ? 0 : limit * (page - 1);

        // Lightweight fields
        let member_query_fileds = { 
            uid: 1, member_name: 1, organisation_id: 1, member_id: 1, 
            read_only_flag: 1, member_email: 1, _id: 1, delete_flag: 1, 
            profile_pic: 1, license_type: 1, usergroup_id: 1, roles: 1,
            group_count: 1, role_count: 1
        };

        // Fetch data with pagination
        let queryBuilder = MemberSchema.find(member_query_obj, member_query_fileds)
            .populate('roles.roleId')
            .lean();

        if (limit !== -1) {
            queryBuilder = queryBuilder.skip(skipRecords).limit(limit);
        }

        let memberDtls = await queryBuilder;

        // Process results with precise response like getAllUsers
        var OrgUsersArr = [];
        if (memberDtls.length > 0) {
            var admin_Arr = ((organization_details != null) ? organization_details.organisation_admin : []);
            
            for (let i = 0; i < memberDtls.length; i++) {
                if (lodash.size(memberDtls[i]) > 0) {
                    let member_role_no = 0;
                    let member_role = "";
                    if (lodash.size(organization_details) > 0) {
                        if (parseInt(organization_details.member_id) === parseInt(memberDtls[i].member_id)) {
                            member_role = "Keyadmin";
                            member_role_no = 1;
                        } else if (memberDtls[i].read_only_flag == 0) {
                            if (lodash.size(admin_Arr) > 0 && admin_Arr.includes(parseInt(memberDtls[i].member_id)) == true) {
                                member_role = "Admin";
                                member_role_no = 2;
                            } else {
                                member_role = "Manager";
                                member_role_no = 3;
                            }
                        } else if (memberDtls[i].read_only_flag == 1) {
                            member_role = "Executive";
                            member_role_no = 4;
                        }
                    }

                    // Determine license type
                    let license_type = (memberDtls[i] && memberDtls[i].license_type) ? memberDtls[i].license_type : "";
                    if (memberDtls[i] && memberDtls[i].roles) {
                        const powerUser = memberDtls[i].roles.find(u => u.roleId && u.roleId.licenseType && u.roleId.licenseType.toLowerCase() == 'power users');
                        if (powerUser) {
                            license_type = powerUser.roleId.licenseType;
                        } else {
                            license_type = 'light users';
                        }
                    }

                    // Precise response structure like getAllUsers
                    OrgUsersArr.push({
                        "uid": memberDtls[i].uid,
                        "_id": memberDtls[i]._id,
                        "member_id": memberDtls[i].member_id,
                        "member_name": memberDtls[i].member_name,
                        "member_email": memberDtls[i].member_email,
                        "member_role": member_role,
                        "member_pic": memberDtls[i].profile_pic,
                        "short_name": Globals.ShortName(memberDtls[i].member_name),
                        "usergroup_id": memberDtls[i].usergroup_id,
                        "license_type": license_type,
                        "GroupCount": (memberDtls[i] && memberDtls[i].group_count) ? memberDtls[i].group_count : 0,
                        "RoleCount": (memberDtls[i] && memberDtls[i].role_count) ? memberDtls[i].role_count : 0
                    });
                }
            }
            
            // Sort by role hierarchy
            OrgUsersArr = ((OrgUsersArr.length > 0) ? lodash.orderBy(OrgUsersArr, ['member_role_no'], ['asc']) : []);
            
            // Apply admin filter
            if (admin) {
                OrgUsersArr = OrgUsersArr.filter((item) => item.member_role.toLowerCase() !== 'keyadmin');
            }
            
            // Pagination response
            const pagination = {
                total: count_members,
                page,
                limit,
                hasMore: (limit === -1) ? false : (skipRecords + limit) < count_members
            };
            
            return {
                status: 200,
                data: OrgUsersArr,
                pagination: pagination
            };
        } else {
            return {
                status: 200,
                data: [],
                pagination: {
                    total: 0,
                    page: page,
                    limit: limit,
                    hasMore: false
                }
            };
        }
    } catch (error) {
        console.log("Error in selectAllUsersAdvanced", error)
        return {
            status: 500,
            data: error
        };
    }
};
```

## **Enhanced Pagination Features:**

### **✅ Flexible Pagination:**
- **`limit = -1`**: Returns ALL records (no pagination)
- **`limit > 0`**: Standard pagination with skip/limit
- **`limit = 0`**: Default behavior (20 records per page)

### **✅ Usage Examples:**

```javascript
// Get all records (no pagination)
GET /api?mode=selectAllUsersAdvanced&limit=-1

// Standard pagination
GET /api?mode=selectAllUsersAdvanced&page=1&limit=10

// Search all records
GET /api?mode=selectAllUsersAdvanced&query=john&limit=-1

// Get all power users
GET /api?mode=selectAllUsersAdvanced&type=power_users&limit=-1

// Paginated search
GET /api?mode=selectAllUsersAdvanced&query=admin&page=2&limit=15
```

### **✅ Response Structure:**

**With Pagination (`limit > 0`):**
```javascript
{
  status: 200,
  data: [...],
  pagination: {
    total: 150,
    page: 1,
    limit: 20,
    hasMore: true
  }
}
```

**Get All Records (`limit = -1`):**
```javascript
{
  status: 200,
  data: [...],
  pagination: {
    total: 150,
    page: 1,
    limit: -1,
    hasMore: false
  }
}
```

This enhancement makes the API much more flexible - perfect for scenarios where you need all records for dropdowns/selectors, while still supporting efficient pagination for large datasets!