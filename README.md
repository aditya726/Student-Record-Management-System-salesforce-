# Student Record Management System using Salesforce (Apex + Visualforce)

A simple Student Record Management Application built using Salesforce, Apex Programming Language, and Visualforce Pages.

The application allows users to:

- Add Student Records
- View Student Records
- Delete Student Records

---

# Technologies Used

- Salesforce
- Apex
- Visualforce
- SOQL

---

# Features

- Create Student Records
- Display Student List
- Delete Student Records
- Custom Object Integration
- Apex Controller Logic
- Visualforce UI

---

# Student Object Fields

| Field Label | API Name | Data Type |
|---|---|---|
| Student Name | Name | Text |
| Roll No | Roll_no__c | Number |
| Class | class__c | Text |
| Phone No | Phone_no__c | Phone |

---

# Step-by-Step Setup Guide

## 1. Login to Salesforce

Open:

https://login.salesforce.com

If you do not have an account:

https://developer.salesforce.com/signup

---

# 2. Open Setup

1. Click the Gear Icon (⚙️)
2. Click **Setup**

---

# 3. Create Custom Object

Navigate to:

Setup → Object Manager → Create → Custom Object

Fill the following details:

| Property | Value |
|---|---|
| Label | Student |
| Plural Label | Students |
| Object Name | Student |
| Record Name | Student Name |
| Data Type | Text |

Enable:

- Allow Reports
- Allow Search
- Track Activities

Click **Save**

---

# 4. Create Custom Fields

Navigate to:

Object Manager → Student → Fields & Relationships → New

---

## A. Roll Number Field

| Property | Value |
|---|---|
| Data Type | Number |
| Field Label | Roll no |
| Length | 5 |
| Decimal Places | 0 |

Generated API Name:

```text
Roll_no__c
```

Save the field.

---

## B. Class Field

| Property | Value |
|---|---|
| Data Type | Text |
| Field Label | class |
| Length | 10 |

Generated API Name:

```text
class__c
```

Save the field.

---

## C. Phone Number Field

| Property | Value |
|---|---|
| Data Type | Phone |
| Field Label | Phone no |

Generated API Name:

```text
Phone_no__c
```

Save the field.

---

# 5. Create Apex Controller

Navigate to:

Setup → Apex Classes → New

Paste the following code:

```apex
public class StudentController {

    public Student__c student {get; set;}
    public List<Student__c> studentList {get; set;}

    public StudentController() {

        student = new Student__c();

        loadStudents();
    }

    // Save Student Record
    public void saveStudent() {

        insert student;

        student = new Student__c();

        loadStudents();
    }

    // Load All Students
    public void loadStudents() {

        studentList = [

            SELECT Id,
                   Name,
                   Roll_no__c,
                   class__c,
                   Phone_no__c

            FROM Student__c

            ORDER BY CreatedDate DESC
        ];
    }

    // Delete Student Record
    public void deleteStudent() {

        Id studentId =
            ApexPages.currentPage()
                     .getParameters()
                     .get('studentId');

        Student__c stu = [

            SELECT Id
            FROM Student__c
            WHERE Id = :studentId
            LIMIT 1
        ];

        delete stu;

        loadStudents();
    }
}
```

Click **Save**

---

# 6. Create Visualforce Page

Navigate to:

Setup → Visualforce Pages → New

Page Name:

```text
StudentManagement
```

Paste the following code:

```html
<apex:page controller="StudentController">

    <apex:form>

        <apex:pageBlock title="Student Record Management">

            <!-- Student Form -->
            <apex:pageBlockSection columns="1">

                <apex:inputField value="{!student.Name}" />

                <apex:inputField value="{!student.Roll_no__c}" />

                <apex:inputField value="{!student.class__c}" />

                <apex:inputField value="{!student.Phone_no__c}" />

                <apex:commandButton value="Save Student"
                                    action="{!saveStudent}"
                                    rerender="studentTable"/>

            </apex:pageBlockSection>

        </apex:pageBlock>

        <!-- Student Table -->
        <apex:pageBlock title="Student List"
                         id="studentTable">

            <apex:pageBlockTable value="{!studentList}"
                                 var="stu">

                <apex:column value="{!stu.Name}"
                             headerValue="Name"/>

                <apex:column value="{!stu.Roll_no__c}"
                             headerValue="Roll No"/>

                <apex:column value="{!stu.class__c}"
                             headerValue="Class"/>

                <apex:column value="{!stu.Phone_no__c}"
                             headerValue="Phone No"/>

                <apex:column headerValue="Action">

                    <apex:commandLink value="Delete"
                                      action="{!deleteStudent}">

                        <apex:param name="studentId"
                                    value="{!stu.Id}" />

                    </apex:commandLink>

                </apex:column>

            </apex:pageBlockTable>

        </apex:pageBlock>

    </apex:form>

</apex:page>
```

Click **Save**

---

# 7. Run the Application

After saving the Visualforce page:

Click:

```text
Preview
```

OR open:

```text
https://yourDomain.visual.force.com/apex/StudentManagement
```

---

# Application Workflow

1. Enter student details
2. Click **Save Student**
3. Record gets stored in the Student object
4. Student list updates automatically
5. Click **Delete** to remove records

---

# Project Structure

```text
Student Record Management System
│
├── Custom Object
│     └── Student__c
│
├── Custom Fields
│     ├── Roll_no__c
│     ├── class__c
│     └── Phone_no__c
│
├── Apex Controller
│     └── StudentController.cls
│
└── Visualforce Page
      └── StudentManagement.page
```

---

# SOQL Query Used

```sql
SELECT Id,
       Name,
       Roll_no__c,
       class__c,
       Phone_no__c
FROM Student__c
ORDER BY CreatedDate DESC
```

---

# Future Enhancements

- Update/Edit Student Records
- Search Functionality
- Pagination
- Validation Rules
- Lightning Web Components (LWC)
- Responsive UI Design

---

# Author

Developed using Salesforce Apex and Visualforce for Student Record Management.
