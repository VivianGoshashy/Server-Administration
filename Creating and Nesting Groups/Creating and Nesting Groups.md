# Project Phase: Creating and Nesting Groups

# Background
With user accounts successfully created and assigned to their respective departments, the next step in NewVue Health’s directory design is to organize users into Active Directory groups.

Groups make it easier to assign permissions, apply policies, and manage access to network resources.
Each department at NewVue Health will have one departmental group that represents the entire team, along with three functional role-based groups corresponding to specific job functions.

The departmental group will serve as the parent group, while the three role-based groups will be nested inside it.
Finally, user accounts will be added as members of the appropriate role-based groups, creating a clear and scalable hierarchy of group membership.

# Purpose of the Assignment
This lab is designed to help students develop practical skills in Active Directory group management—a fundamental responsibility of a System Administrator.

By performing this activity, students will:

- Learn how to create and organize security groups that mirror real-world departmental structures.

- Understand how group nesting enhances security design by reducing administrative overhead and centralizing access control.

- Gain experience using both graphical tools (ADUC) and command-line automation (PowerShell) to accomplish the same administrative task—an essential skill in modern hybrid environments.

- Build the ability to manage role-based access control (RBAC), ensuring users receive permissions based on job function rather than individual assignment.

Mastering these skills prepares students to manage enterprise-scale Active Directory environments, design efficient group hierarchies, and implement best practices for security and permissions management.

# NewVue Health – Group Structure

| **Department** |	**Departmental Group**	| **Role-Based Groups** |
| :---: | :---: | :---: |
| Administration	| Administration |	Admin_Managers, Admin_Clerks, Admin_Executives |
| Clinical Services	| Clinical_Services	| Clinical_Doctors, Clinical_Nurses, Clinical_Assistants | 
| Human Resources | 	Human_Resources	| HR_Managers, HR_Recruiters, HR_Assistants |
| IT Operations |	IT_Operations	| IT_Network, IT_Security, IT_Applications | 
| Finance	| Finance	| Finance_Accountants, Finance_Auditors, Finance_Analysts |

Newvue Health Group Structure

# Tasks and Steps to Complete the Activity
# Task 1 – Creating Groups for Administration Department (Using ADUC)
## Task 1 – Creating Groups for Administration Department (Using ADUC)
1. Log on to NV-DC1 as newvue\Administrator.

2. Open Server Manager → Tools → Active Directory Users and Computers (ADUC).

3. Expand newvue.local → Administration → Functional Units.

4. Right-click the Functional Units OU → New → Group.

5. Create the following groups:

- Administration (departmental group)

- Admin_Managers

- Admin_Clerks

- Admin_Executives

6. Ensure all groups are created with:

- Group scope: Global

- Group type: Security

7. Verify that all four groups appear under Functional Units.

Evidence 1: Screenshot of the New Object – Group wizard showing the Administration group creation.
Evidence 2: Screenshot of ADUC → Administration → Functional Units showing all four groups.

## Task 2 – Creating Groups for Clinical Services Department (Using ADUC)
## Task 2 – Creating Groups for Clinical Services Department (Using ADUC)
1. Expand newvue.local → Clinical Services → Functional Units.

2. Right-click Functional Units → New → Group.

3. Create the following groups:

- Clinical_Services

- Clinical_Doctors

- Clinical_Nurses

- Clinical_Assistants

4. Use Global scope and Security type for each group.

5. Verify that all four groups appear under Functional Units.

Evidence 3: Screenshot of New Object – Group wizard showing Clinical_Services group creation.
Evidence 4: Screenshot of ADUC → Clinical Services → Functional Units showing all four groups.

## Task 3 – Creating Groups for Human Resources Department (Using ADUC)
1. Expand newvue.local → Human Resources → Functional Units.

2. Right-click Functional Units → New → Group.

3. Create the following groups:

- Human_Resources

- HR_Managers

- HR_Recruiters

- HR_Assistants

4. Ensure all groups have Global scope and Security type.

5. Confirm that all groups appear under Functional Units.

Evidence 5: Screenshot of New Object – Group wizard showing Human_Resources group creation.
Evidence 6: Screenshot of ADUC → Human Resources → Functional Units showing all four groups.

## Task 4 – Creating Groups for IT Operations Department (Using ADUC)
1. Expand newvue.local → IT Operations → Functional Units.

2. Right-click Functional Units → New → Group.

3. Create the following groups:

- IT_Operations

- IT_Network

- IT_Security

- IT_Applications

4. Confirm all groups use Global scope and Security type.

5. Verify that all four groups appear in Functional Units.

Evidence 7: Screenshot of New Object – Group wizard showing IT_Operations group creation.
Evidence 8: Screenshot of ADUC → IT Operations → Functional Units showing all four groups.

## Task 2 - Creating Groups for Finance Department (Using PowerShell)
1. Open Windows PowerShell as Administrator.

2. Run the following commands:

3. New-ADGroup -Name "Finance" -GroupScope Global -GroupCategory Security -Path "OU=Functional Units,OU=Finance,DC=newvue,DC=local"

4. New-ADGroup -Name "Finance_Accountants" -GroupScope Global -GroupCategory Security -Path "OU=Functional Units,OU=Finance,DC=newvue,DC=local"

5. New-ADGroup -Name "Finance_Auditors" -GroupScope Global -GroupCategory Security -Path "OU=Functional Units,OU=Finance,DC=newvue,DC=local"

6. New-ADGroup -Name "Finance_Analysts" -GroupScope Global -GroupCategory Security -Path "OU=Functional Units,OU=Finance,DC=newvue,DC=local"

7. Open ADUC → Finance → Functional Units and confirm the groups exist.

Evidence 9: Screenshot of PowerShell showing successful creation of Finance groups.
Evidence 10: Screenshot of ADUC → Finance → Functional Units showing all four groups.

## Task 3 - Nesting Role-Based Groups into Departmental Groups
In this task, you will nest the three role-based groups of each department as members of the main departmental group.
This structure simplifies permission management by ensuring that access assigned to the departmental group automatically applies to all nested role-based groups.

### Nesting for Administration Department
1. In ADUC, navigate to:
### newvue.local → Administration → Functional Units.

2. Right-click the Administration group → Properties → Members → Add.

4. Click Advanced → Find Now and select the following groups:

- Admin_Managers

- Admin_Clerks

- Admin_Executives

4. Click OK → Apply → OK to save changes.

5. Confirm that the three role-based groups now appear under the Members tab of the Administration group.

Evidence 11: Screenshot of the Administration → Members tab showing Admin_Managers, Admin_Clerks, and Admin_Executives nested.

### Nesting for Clinical Services Department
### Nesting for Clinical Services Department
1. In ADUC, navigate to:
### newvue.local → Clinical Services → Functional Units.

2. Right-click the Clinical_Services group → Properties → Members → Add.

3. Select and add the following groups:

- Clinical_Doctors

- Clinical_Nurses

- Clinical_Assistants

4. Click OK → Apply → OK.

5. Verify that all three groups appear in the Members tab.

Evidence 12: Screenshot of the Clinical_Services → Members tab showing the three nested groups.

### Nesting for Human Resources Department
1. In ADUC, navigate to:
### newvue.local → Human Resources → Functional Units.

2. Right-click the Human_Resources group → Properties → Members → Add.

3. Add the following groups:

- HR_Managers

- HR_Recruiters

- HR_Assistants

4. Apply and confirm that all three groups appear as members of Human_Resources.

Evidence 13: Screenshot of the Human_Resources → Members tab showing HR_Managers, HR_Recruiters, and HR_Assistants.

### Nesting for IT Operations Department
1. In ADUC, navigate to:
### newvue.local → IT Operations → Functional Units.

2. Right-click IT_Operations → Properties → Members → Add.

3. Add the following groups:

- IT_Network

- IT_Security

- IT_Applications

4. Save and verify the three groups appear in the Members list.

Evidence 14: Screenshot of IT_Operations → Members tab showing IT_Network, IT_Security, and IT_Applications nested.

### Nesting for Finance Department
1. In ADUC, navigate to:
### newvue.local → Finance → Functional Units.

2. Right-click Finance → Properties → Members → Add.

3. Add the following groups:

- Finance_Accountants

- Finance_Auditors

- Finance_Analysts

4. Save and verify that all three groups appear under the Members tab.

Evidence 15: Screenshot of Finance → Members tab showing Finance_Accountants, Finance_Auditors, and Finance_Analysts.

## Task 4 - Adding Users to Role-Based Groups
With all departmental and role-based groups created and nested, the next step is to populate each role-based group with users.
This ensures that permissions are granted based on functional responsibilities rather than individual accounts — an essential principle of Role-Based Access Control (RBAC).

In this exercise, you will evenly distribute users among the various role-based groups in each department.
While the exact users may differ for each student, the goal is to demonstrate that you understand how to assign users to the correct groups and verify the relationships in Active Directory Users and Computers (ADUC).

### Administration Department
Within Administration → Functional Units, locate the following role-based groups:

- Admin_Managers

- Admin_Clerks

- Admin_Executives

Evenly assign a few users from Administration → Users to each of these groups.

1. In ADUC, right-click a user account → Add to a group …

2. Enter or browse for the target group (e.g., Admin_Managers) and click OK.

3. Repeat for additional users so that each group has at least one or two members.

4. Verify each user’s membership via Properties → Member Of tab.

Evidence : Screenshot of the members tab of the following groups

- Admin_Managers,
- Admin_Clerks,
- Admin_Executives .

### Clinical Services Department
Within Clinical Services → Functional Units, assign users to:

- Clinical_Doctors

- Clinical_Nurses

- Clinical_Assistants

Distribute users from the Clinical Services → Users OU evenly among the three groups.

 

Evidence : Screenshot of the Members tab of the following groups:

- Clinical_Doctors

- Clinical_Nurses

- Clinical_Assistants

### Human Resources Department
Within Human Resources → Functional Units, assign users to:

- HR_Managers

- HR_Recruiters

- HR_Assistants

Ensure that users are assigned logically — for example, a few to each group — to simulate a real HR environment.

Evidence : Screenshot of the Members tab of the following groups:

- HR_Managers

- HR_Recruiters

- HR_Assistants

### IT Operations Department
Within IT Operations → Functional Units, assign users to:

- IT_Network

- IT_Security

- IT_Applications

Distribute existing IT user accounts evenly across these three groups.

Evidence: Screenshot of the Members tab of the following groups:

- IT_Network

- IT_Security

- IT_Applications

### Finance Department
Within Finance → Functional Units, assign users to:

- Finance_Accountants

- Finance_Auditors

- Finance_Analysts

 

Assign available Finance users evenly among the groups. Confirm each group’s membership using the Members tab.

Evidence 20: Screenshot of the Members tab of the following groups:

- Finance_Accountants

- Finance_Auditors

- Finance_Analysts
