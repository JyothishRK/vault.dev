 # Key Features Comparison: `organisationUsersList` vs `selectAllUsers`

## **1. organisationUsersList** - Comprehensive User Management API

### **Filters Available:**
- **User Type**: `active`, `inactive`, `power_users`, `light_users`, `admin`, `manager`, `executive`, `risk_oversight`
- **Search**: Prefix search (`a`), full-text search (`q`)
- **Pagination**: Page number (`p`), pagination type (`small`/`medium`/`large`)
- **Permission**: Permission-based filtering (`permType`)
- **Risk Context**: Risk-specific filtering (`riskid`)
- **Admin Filter**: Exclude KeyAdmin (`admin=1`)

### **RBAC (Role-Based Access Control):**
```javascript
// Role Hierarchy (1=Highest, 4=Lowest)
1. KeyAdmin - Full access
2. Admin - Can manage Managers/Executives
3. Manager - Can view Executives only
4. Executive - Read-only access

// Permission Checks:
- View Details: Based on role hierarchy
- Login Override: KeyAdmin/Admin only
- User Actions: Role-based enable/disable flags
- Risk Oversight: Excludes risk owners/heads
```

### **Response Structure:**
```javascript
{
  status: 200,
  data: [
    {
      // Core Info
      member_id, member_name, member_email, status,
      member_role, member_role_no,
      
      // Permissions & Actions
      loginEnable, RTLEnable, DeleteEnable,
      CTMEnable, CTAEnable, CTKAEnable, SALEnable,
      
      // Detailed Data
      GroupCount, RoleCount, GroupDetails, RolesDetails,
      license_type, MFAEnabled, isFaceIdEnabled,
      employee_performance, report_count, totalReportCount,
      
      // Profile & Status
      profile_pic, last_password_updated_utc,
      exceed_user, inProgress
    }
  ]
}
```

---

## **2. selectAllUsers** - Lightweight User Selection API

### **Filters Available:**
- **User Type**: `all`, `policySearch`, default (non-readonly)
- **Permission**: Permission-based filtering (`permType`)
- **Admin Filter**: Exclude KeyAdmin (`admin=1`)

### **RBAC (Role-Based Access Control):**
```javascript
// Simplified Role Determination:
1. KeyAdmin - Organization owner
2. Admin - In organisation_admin array
3. Manager - Non-readonly users
4. Executive - Readonly users

// Access Control:
- No detailed permission checks
- Basic role-based filtering
- Permission-based user filtering via roles/groups
```

### **Response Structure:**
```javascript
// Default Response:
{
  status: 200,
  data: [
    {
      uid, _id, member_id, member_name, member_email,
      member_role, member_pic, short_name, usergroup_id
    }
  ]
}

// Policy Search Response:
{
  status: 200,
  data: {
    success: true,
    statusCode: 200,
    statusMessage: "Organization users fetched successfully",
    data: {
      users: [
        {
          uid, id, name, email, type, imageUrl
        }
      ]
    }
  }
}
```

---

## **Key Differences Summary:**

| Feature              | organisationUsersList    | selectAllUsers         |
| -------------------- | ------------------------ | ---------------------- |
| **Purpose**          | Full user management     | User selection/picker  |
| **Filters**          | 8+ filter types + search | 3 basic filters        |
| **Pagination**       | ✅ Full pagination        | ❌ No pagination        |
| **Search**           | ✅ Advanced search        | ❌ No search            |
| **RBAC**             | ✅ Detailed permissions   | ✅ Basic role filtering |
| **Response Size**    | Large (30+ fields)       | Small (8-10 fields)    |
| **Performance Data** | ✅ Included               | ❌ Not included         |
| **Action Flags**     | ✅ 10+ action flags       | ❌ No action flags      |
| **Use Case**         | User management pages    | Dropdowns, pickers     |

**Bottom Line**: `organisationUsersList` is a full-featured user management API with comprehensive filtering and detailed responses, while `selectAllUsers` is a lightweight API optimized for simple user selection scenarios.


---


I'll remove the `limit` parameter and keep only the `paginationType` approach. Let me update the implementation:


[4 tools called]

Perfect! I've successfully removed the `limit` parameter. Now let me verify the updated implementation:


[1 tool called]

Excellent! ✅ **The `limit` parameter has been successfully removed.**

## Updated Implementation Summary:

### **Current Behavior:**
- **Only `paginationType` controls page size** - no more `limit` parameter
- **Default**: `small` pagination (30 items)
- **Available types**: `small` (30), `medium` (50), `large` (100)
- **Config-based**: Uses environment variables for limits

### **Updated API Parameters Table:**

| Parameter        | Type      | Default | Required | Description                                                          |
| ---------------- | --------- | ------- | -------- | -------------------------------------------------------------------- |
| `page`           | `integer` | `1`     | `No`     | Page number for pagination                                           |
| `paginationType` | `string`  | `small` | `No`     | Pagination size: `small` (30), `medium` (50), `large` (100)          |
| `query`          | `string`  | `-`     | `No`     | Search term for name/email                                           |
| `type`           | `string`  | `-`     | `No`     | User type filter (active, inactive, power_users, light_users, admin) |
| `admin`          | `integer` | `0`     | `No`     | Exclude KeyAdmin (0/1)                                               |
| `permType`       | `string`  | `-`     | `No`     | Permission-based filter                                              |

### **Usage Examples:**
```javascript
// Default behavior (small pagination, 30 items)
GET /api/users?page=1

// Medium pagination (50 items)
GET /api/users?page=1&paginationType=medium

// Large pagination (100 items)
GET /api/users?page=1&paginationType=large
```

The implementation is now cleaner and more consistent - only `paginationType` controls the page size, making it simpler for API consumers to use.