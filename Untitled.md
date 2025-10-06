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


# Parameters for `selectAllUsersAdvanced` Endpoint

## **Core Parameters:**

### **Pagination Parameters:**
- **`page`** (integer, default: 1)
  - Page number for pagination
  - Starts from 1
  - Example: `page=2`

- **`limit`** (integer, default: 20)
  - Number of records per page
  - `-1` = Get all records (no pagination)
  - `> 0` = Standard pagination
  - Example: `limit=10`, `limit=-1`

### **Search Parameter:**
- **`query`** (string, optional)
  - Search term for user name or email
  - Supports regex patterns
  - Case-insensitive search
  - Example: `query=john`, `query=admin@company.com`

### **Filter Parameters:**
- **`type`** (string, optional)
  - User type filter
  - Available values:
    - `active` - Active users only
    - `inactive` - Inactive users only
    - `power_users` - Power users only
    - `light_users` - Light users only
    - `admin` - Organization admins only
    - `manager` - Non-admin managers only
    - `executive` - Read-only executives only
    - `all_except_admin` - All users except admins
    - `all_except_executive` - All users except executives
  - Example: `type=active`, `type=power_users`

- **`admin`** (integer, optional, default: 0)
  - Admin filter flag
  - `0` = Include all users (including KeyAdmin)
  - `1` = Exclude KeyAdmin users
  - Example: `admin=1`

- **`permType`** (string, optional)
  - Permission-based filtering
  - Filters users based on assigned roles and groups
  - Example: `permType=responsibility center`, `permType=policies`

## **Parameter Usage Examples:**

### **Basic Usage:**
```javascript
// Get first page with default limit (20)
GET /api?mode=selectAllUsersAdvanced

// Get specific page with custom limit
GET /api?mode=selectAllUsersAdvanced&page=2&limit=15

// Get all records (no pagination)
GET /api?mode=selectAllUsersAdvanced&limit=-1
```

### **Search Usage:**
```javascript
// Search by name/email
GET /api?mode=selectAllUsersAdvanced&query=john

// Search with pagination
GET /api?mode=selectAllUsersAdvanced&query=admin&page=1&limit=10

// Search all records
GET /api?mode=selectAllUsersAdvanced&query=manager&limit=-1
```

### **Filter Usage:**
```javascript
// Get active users only
GET /api?mode=selectAllUsersAdvanced&type=active

// Get power users with pagination
GET /api?mode=selectAllUsersAdvanced&type=power_users&page=1&limit=25

// Get managers excluding KeyAdmin
GET /api?mode=selectAllUsersAdvanced&type=manager&admin=1

// Get all users except admins
GET /api?mode=selectAllUsersAdvanced&type=all_except_admin
```

### **Combined Usage:**
```javascript
// Search active users with pagination
GET /api?mode=selectAllUsersAdvanced&type=active&query=john&page=1&limit=20

// Get power users with permission filtering
GET /api?mode=selectAllUsersAdvanced&type=power_users&permType=responsibility center

// Search managers excluding KeyAdmin
GET /api?mode=selectAllUsersAdvanced&type=manager&query=admin&admin=1&page=1&limit=15

// Get all inactive users
GET /api?mode=selectAllUsersAdvanced&type=inactive&limit=-1
```

## **Parameter Summary Table:**

| Parameter  | Type    | Default | Required | Description                   |
| ---------- | ------- | ------- | -------- | ----------------------------- |
| `page`     | integer | 1       | No       | Page number for pagination    |
| `limit`    | integer | 20      | No       | Records per page (-1 for all) |
| `query`    | string  | -       | No       | Search term for name/email    |
| `type`     | string  | -       | No       | User type filter              |
| `admin`    | integer | 0       | No       | Exclude KeyAdmin (0/1)        |
| `permType` | string  | -       | No       | Permission-based filter       |

## **Response Structure:**
```javascript
{
  status: 200,
  data: [
    {
      uid: "unique_id",
      _id: "mongo_id",
      member_id: "member_id",
      member_name: "User Name",
      member_email: "user@email.com",
      member_role: "Keyadmin|Admin|Manager|Executive",
      member_pic: "profile_pic_url",
      short_name: "UN",
      usergroup_id: ["group1", "group2"],
      license_type: "power users|light users",
      GroupCount: 2,
      RoleCount: 3
    }
  ],
  pagination: {
    total: 150,
    page: 1,
    limit: 20,
    hasMore: true
  }
}
```

This endpoint provides comprehensive filtering and search capabilities with flexible pagination options!