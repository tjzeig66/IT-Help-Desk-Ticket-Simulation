
# Ticket 001: Shared Network Drive Permission Issue

## Project Overview

This project simulates a real-world IT help desk incident using Jira Service Management, Active Directory, Windows Server 2022, and Windows 11 Pro.

The objective is to investigate a network share permission issue, identify the cause, implement an appropriate solution, and document the troubleshooting process.

**Ticket Type:** Incident  
**Priority:** Medium  
**Status:** In Progress  
**Affected User:** Mickey Mouse  
**Department:** Human Resources  
**Job Title:** HR Specialist  
**Domain:** tjzeig.local  
**Network Drive:** Shared (S:)

> **Note:** All employee accounts and support requests are fictional and created for educational purposes.

---

## 1. Lab Environment

| Component | Description |
|---|---|
| Hypervisor | VMware Workstation |
| Domain Controller | Windows Server 2022 |
| Client Workstation | Windows 11 Pro |
| Domain | tjzeig.local |
| Directory Services | Active Directory Domain Services |
| Ticketing System | Jira Service Management |
| Network Share | SHARED |
| Shared Folder Location | C:\SHARED |
| Mapped Network Drive | S: |
| HR Security Group | US-SG-HR |

The Windows 11 workstation is joined to the `tjzeig.local` domain.

The shared network drive is configured through Group Policy.

---

## 2. Incident Description

An HR Specialist reported being unable to create folders on the Shared (S:) network drive.

The employee requires access to organize onboarding documents.

### Reported Error

"You need permission to perform this action."


### Screenshot: Shared Drive Permission Error

The employee attempted to create a new folder on the Shared (S:) network drive but received the following Windows error:

"You need permission to perform this action."

The screenshot below documents the permission error encountered during the incident.

[Shared Drive Permission Error](screenshots/Ticket-001-Permission-Error.png)


### Business Impact

The employee is unable to create folders needed to organize HR onboarding documentation.

### Expected Result

The employee should be able to create folders within an authorized location on the shared network drive.

---

## 3. Jira Ticket Creation

Created an incident in Jira Service Management to simulate an employee submitting a technical support request.

### Ticket Summary

Unable to Create Folder on Shared (S:) Drive – HR Department

### Employee Request

Hi,

I'm having trouble creating folders on the Shared (S:) network drive. I can open the drive and view its contents, but whenever I try to create a new folder, I receive an error saying, "You need permission to perform this action."

I need to create a folder to organize employee onboarding documents, but this issue is preventing me from doing so.

Could someone from IT please look into this?

Thanks,  
Mickey Mouse  
HR Specialist

### Ticket Management

- Created the incident in Jira Service Management.
- Assigned the ticket to the IT support technician.
- Changed the ticket status to In Progress.
- Began investigating the reported issue.

**Screenshot:**

[Jira Incident Ticket](screenshots/Ticket-001-Jira-Incident.png)

---

## 4. Troubleshooting: Locating the Shared Folder

### Initial Investigation

Accessed the Windows Server 2022 domain controller and opened File Explorer to locate the shared folder.

The folder was not immediately found in the location initially inspected.

### Additional Investigation

Opened:

Server Manager → File and Storage Services → Shares

Located the network share named `SHARED`.

Confirmed the local folder path:

`C:\SHARED`

### Findings

The shared folder was configured on Windows Server 2022, and its physical location was identified.

**Screenshot:**

[Shared Folder Location](screenshots/File-Shared-Located.png)

---

## 5. Troubleshooting: NTFS Permissions

Navigated to:

`C:\SHARED`

Right-clicked the folder and selected:

Properties → Security

### Existing Security Entries

The following entries were visible:

- SYSTEM
- TJZEIG\Administrator
- TJZEIG\Users

### Permissions for TJZEIG\Users

| Permission | Allowed |
|---|---|
| Full Control | No |
| Modify | No |
| Read & Execute | Yes |
| List Folder Contents | Yes |
| Read | Yes |
| Write | No |

### Findings

The inspected Users group entry granted read-related permissions but did not grant Write or Modify permissions.

This was consistent with the reported inability to create folders.

Additional permissions and group memberships still needed to be considered before confirming the effective permissions for the affected user.

**Screenshot:**

[NTFS Permissions](screenshots/Ticket-001-NTFS-Permissions.png)

---

## 6. Troubleshooting: Share Permissions

Opened:

C:\SHARED → Properties → Sharing → Advanced Sharing → Permissions

### Existing Share Permissions

| Group | Read | Change | Full Control |
|---|---|---|---|
| Everyone | Allow | Not allowed | Not allowed |
| Domain Users | Allow | Not allowed | Not allowed |

### Findings

The inspected share permissions allowed reading but did not grant Change or Full Control to either group.

Windows network file access is governed by both share permissions and NTFS permissions.

Even if NTFS permissions allow writing, restrictive share permissions can prevent modifications over the network.

### Preliminary Assessment

The observed share permissions are consistent with the inability to create folders.

The appropriate access scope must be confirmed before modifying permissions.

**Screenshot:**

[Share Permissions](screenshots/Ticket-001-Share-Permissions.png)

---

## 7. Access Control Considerations

The affected employee belongs to the Human Resources department.

An existing Active Directory security group, `US-SG-HR`, represents HR users.

Before granting access, the following considerations were identified:

- Confirm that the employee is authorized to create folders.
- Determine whether access should be individual or group-based.
- Avoid granting unnecessary write access to all Domain Users.
- Apply the principle of least privilege.
- Consider a dedicated HR folder rather than granting write access to the entire shared drive.

### Proposed Solution

Configure appropriate share and NTFS permissions for the authorized user or group.

The exact permission changes will be documented after implementation.

---

## 8. Resolution

**Status: Pending**

The final resolution has not yet been implemented and verified.

Once completed, document:

1. The approved access requirements.
2. The share permission changes.
3. The NTFS permission changes.
4. The account or security group receiving access.
5. Any additional troubleshooting performed.

**Screenshot:**

_Add screenshot of the final permission configuration._

---

## 9. Verification

**Status: Pending**

After implementing the approved permission changes:

1. Sign in to Windows 11 using the affected employee's domain account.
2. Open File Explorer.
3. Navigate to Shared (S:).
4. Open the authorized folder.
5. Attempt to create a new test folder.
6. Verify that the operation succeeds.
7. Confirm that unrelated users have not received unnecessary write access.

**Screenshot:**

_Add screenshot showing successful folder creation._

---

## 10. Jira Resolution Documentation

**Status: Pending**

Once the issue has been resolved:

- Add an internal note documenting the root cause.
- Record the troubleshooting and configuration changes.
- Document successful verification.
- Reply to the simulated employee with the outcome.
- Change the Jira ticket status to Resolved.

**Screenshot:**

_Add screenshot of the resolved Jira incident._

---

## 11. Skills Demonstrated

- Jira Service Management
- IT incident management
- Windows Server 2022 administration
- Windows 11 troubleshooting
- Active Directory
- Network drive mapping
- SMB share permissions
- NTFS permissions
- Access control and least privilege
- Technical troubleshooting
- Incident documentation

---

## 12. Key Takeaways

This exercise demonstrates how a help desk technician can investigate a network drive access issue by reviewing the underlying server configuration and permission settings.

The investigation identified restrictive permissions at both the share and NTFS layers.

The exercise also emphasizes the importance of verifying access requirements before modifying permissions and documenting technical findings throughout the incident lifecycle.

**Final outcome:** Pending resolution and verification.

