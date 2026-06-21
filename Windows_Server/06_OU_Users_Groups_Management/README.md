# Lab 06 — Organisational Units, Users & Groups Management

**Topics:** OU · AD Users · Security Groups · Group Scope · ADUC · Domain Join Verification

---

## Objective

Design and implement an Organisational Unit (OU) structure in the `mylab.local` domain
that mirrors a company department layout. Create user accounts, assign them to security
groups, and verify domain-joined client access.

---

## Environment

| Component | Detail |
|-----------|--------|
| Domain | mylab.local |
| DC Hostname | WS2K19-DC01 |
| Client | Oprekin-PC.mylab.local |

---

## Key Concepts

**Organisational Unit (OU)** is a container in AD that organises users, computers, and
groups logically. GPOs are linked to OUs — so different departments can have different
policies applied automatically.

**Security Group** is used to assign permissions to resources (shared folders, printers).
Grant access to the group, not individual users — when someone joins or leaves a team,
you just update group membership.

**Group Scope:**
- **Global** — holds users from the same domain; standard for department role groups
- **Domain Local** — grants access to resources in the same domain
- **Universal** — used across multiple domains in a forest

---

## OU Structure Design

```
mylab.local
└── _MYLAB                          ← root company OU
    ├── Departments
    │   ├── Sales
    │   │   ├── Users
    │   │   └── Computers
    │   ├── IT
    │   │   ├── Users
    │   │   └── Computers
    │   └── HR
    │       ├── Users
    │       └── Computers
    └── Groups
        └── Security Groups
```

> **Why a custom root OU?** The built-in AD containers (like `Users`) cannot have
> GPOs linked to them. A custom OU structure gives full GPO control per department.
> The `_` prefix keeps it at the top of the AD tree alphabetically.

---

## Configuration Steps

---

### STEP 1 — Create the OU Structure

```
Active Directory Users and Computers (ADUC)
→ Right-click mylab.local → New → Organizational Unit
  Name: _MYLAB

Continue creating sub-OUs:
  _MYLAB → Departments
  Departments → Sales, IT, HR
  Sales → Users, Computers
  (repeat for IT and HR)
  _MYLAB → Groups → Security Groups
```

---

### STEP 2 — Create User Accounts

```
ADUC → Sales → Users → Right-click → New → User

Example:
  First name  : John
  Last name   : Sales
  User logon  : j.sales@mylab.local
  Password    : P@ssword123
  ☑ User must change password at next logon
```

Repeat for HR and IT department users.

> Always place users inside their department OU — not in the default `Users`
> container — so that department-specific GPOs apply to them automatically.

---

### STEP 3 — Create Security Groups

```
ADUC → Groups → Security Groups → Right-click → New → Group

Group name   : GRP_Sales
Group scope  : Global
Group type   : Security

Repeat:
  GRP_IT
  GRP_HR
```

---

### STEP 4 — Add Users to Groups

```
Right-click GRP_Sales → Properties → Members → Add
→ Type j.sales → Check Names → OK

Repeat for each user and their respective group.
```

Verify:
```
Right-click GRP_Sales → Properties → Members
→ John Sales should appear
```

---

### STEP 5 — Move Computer Account to Correct OU

When a machine joins the domain, Windows places it in the default `Computers`
container. Move it to the correct department OU:

```
ADUC → Computers → Right-click Oprekin-PC → Move
→ Select: _MYLAB → Departments → IT → Computers → OK
```

> This ensures Computer Configuration GPOs linked to the IT OU apply to Oprekin-PC.

---

### STEP 6 — Verify Domain Join on Client

The client machine `Oprekin-PC` was verified as successfully joined to `mylab.local`
via System Properties.

---

## Screenshots

### Windows 10 Client — Domain Join Confirmed

![Client Domain Join](../02_Active_Directory_Domain_Setup/screenshots/client-domain-join.png)

> System Properties on Oprekin-PC showing:
> Full computer name: `Oprekin-PC.mylab.local`
> Domain: `mylab.local`
> This confirms the client is a member of the domain and will receive GPOs and
> authenticate against WS2K19-DC01.

---

## Verification Summary

| Check | Method | Expected |
|-------|--------|----------|
| OUs created | ADUC tree | _MYLAB, Departments, Sales, IT, HR visible |
| Users in correct OU | ADUC → Sales → Users | j.sales listed |
| Group membership | GRP_Sales → Properties → Members | j.sales listed |
| Computer in domain | Client System Properties | Domain: mylab.local |
| Computer in correct OU | ADUC → IT → Computers | Oprekin-PC listed |

---

## Common Issues & Fixes

| Problem | Cause | Fix |
|---------|-------|-----|
| GPO not applying to user | User in wrong OU or default container | Move user to correct department OU |
| New group membership not working | Token not refreshed | User must log out and log back in |
| Computer account in wrong OU | Auto-placed in default Computers container | Move manually in ADUC |

---

## Lessons Learned

- Never place users in the default `Users` container — GPOs cannot be linked to it
- Global security groups are the correct scope for department role groups in a single domain
- Moving the computer account to the correct OU is often forgotten — GPOs won't apply until this is done
- OU design should mirror the business structure, not the technical structure
- Group membership changes require a new login session to take effect — the user must log out and back in
