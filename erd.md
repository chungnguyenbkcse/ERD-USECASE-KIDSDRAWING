# ERD diagrams

## 1. User service (Teacher, Staff, Children Service)

```plantuml
@startuml user_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    * id: uuid <<PK>>
    --
    parent_id: uuid <<FK>>
    * username: varchar(255)
    email: varchar(255)
    * password: varchar(255)
    * status: boolean 
    first_name: varchar(255)
    last_name: varchar(255)
    sex: char(1)
    phone: varchar(15)
    address: varchar(255)
    date_of_birth: date
    profile_image_url: varchar(255)
    * deleted: boolean
    * create_time: timestamp
    * update_time: timestamp
}

entity "PASSWORD_RESET_TOKEN" as password_reset_token {
    * id: uuid <<PK>>
    --
    * user_id: uuid <<FK>>
    * token: varchar(255)
    * expiryDate: timestamp
}

entity "ROLE" as role {
    * id: uuid <<PK>>
    --
    * name: varchar(255)
    * description: text
    * create_time: timestamp
    * update_time: timestamp
}

entity "PRIVILEGE" as privilege {
    * id: uuid <<PK>>
    --
    * name: varchar(255)
    * description: text
}

entity "USER_HAS_ROLE" as user_has_role {
    * user_id: uuid <<PK,FK>>
    * role_id: uuid <<PK,FK>>
}

entity "ROLE_HAS_PRIVILEGE" as role_has_privilege {
    * role_id: uuid <<PK,FK>>
    * privilege_id: uuid <<PK,FK>>
}

' Authenticatin Relationship

user ||..o{ user: is\n parent\n of
user_has_role }o--|| user
password_reset_token ||..|| user
user_has_role }o--|| role
role_has_privilege }o--|| role
role_has_privilege }o--|| privilege
@enduml
```

## 3. Art service

```plantuml
@startuml art_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "ART_LEVEL" as art_level {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
}

entity "ART_AGE" as art_age {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
}

entity "ART_TYPE" as art_type {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
}

' Art Relationship

user ||..o{ art_level: create level
user ||..o{ art_type: create type
user ||..o{ art_age: create age
@enduml
```

## 4. Contest service

```plantuml
@startuml contest_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "ART_AGE" as art_age {
    @ref art_service
}

entity "ART_TYPE" as art_type {
    @ref art_service
}

entity "CONTEST" as contest {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * art_age_id: uuid <<FK>>
    * art_type_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * max_participant: integer
    * registration_time: timestamp
    image_url: varchar(255)
    * start_time: timestamp
    * end_time: timestamp
    * create_time: timestamp
    * update_time: timestamp
    * is_enabled: boolean
}

entity "USER_REGISTER_JOIN_CONTEST" as user_register_join_contest {
    * id: uuid <<PK>>
    --
    * student_id: uuid <<FK>>
    * contest_id: uuid <<FK>>
}

entity "USER_GRADE_CONTEST" as user_grade_contest {
    * id: uuid <<PK>>
    --
    * teacher_id: uuid <<FK>>
    * contest_id: uuid <<FK>>
}

' Contest Relationship

user ||..o{ contest: create
user ||..o{ user_register_join_contest: join
user_register_join_contest }o..|| contest
contest }o..|| art_age
contest }o..|| art_type
user ||..o{ user_grade_contest: grade
user_grade_contest }o..|| contest
@enduml
```

## 5. Course service

```plantuml
@startuml course_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "ART_LEVEL" as art_level {
    @ref art_service
}

entity "ART_TYPE" as art_type {
    @ref art_service
}

entity "ART_AGE" as art_age {
    @ref art_service
}

entity "TEACHER_REGISTER_QUALIFICATIONS" as teacher_register_qualification {
    * id: uuid <<PK>>
    --
    * teacher_id: uuid <<FK>>
    * reviewer_id: uuid <<FK>>
    * course_id: uuid <<FK>>
    * degree_photo_url: varchar(255)
    * status: string
}

entity "COURSE" as course {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * art_level_id: uuid <<FK>>
    * art_type_id: uuid <<FK>>
    * art_age_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * num_of_section: integer
    * price: float8
    image_url: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
    * is_enabled: boolean
}


' Course Relationship

user ||..o{ course: create course
user ||..o{ teacher_register_qualification: teach
user ||..o{ teacher_register_qualification: review
course ||..o{ teacher_register_qualification
course }o..|| art_level
course }o..|| art_type
course }o..|| art_age
@enduml
```


## 7. Semester service

```plantuml
@startuml semester_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "COURSE" as course {
    @ref course_service
}

entity "SEMESTER" as semester_creation {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * number: integer 
    * year: integer
    * name: varchar(255)
    * description: text
    * start_time: timestamp
    * end_time: timestamp
    * create_time: timestamp
    * update_time: timestamp
}

entity "HOLIDAY" as holiday {
    * id: uuid <<PK>>
    --
    * semester_id: uuid <<FK>>
    * day: datetime
}

entity "SEMESTER_CLASS" as semester_class {
    * id: uuid <<PK>>
    --
    * semester_id: uuid <<FK>>
    * course_id: uuid <<FK>>
    * name: varchar(255)
    * max_participant: integer
    * registration_time: timestamp
    * registration_expiration_time: timestamp
}

entity "SCHEDULE" as schedule {
    * id: uuid <<PK>>
    --
    * lesson_time: uuid <<FK>>
    * semester_class_id: uuid <<FK>>
    * date_of_week: integer
}

entity "LESSON_TIME" as lesson_time {
    * id: uuid <<PK>>
    --
    * start_time: time
    * end_time: time
}


entity "USER_REGISTER_JOIN_SEMESTER" as user_register_join_class {
    * id: uuid <<PK>>
    --
    * student_id: uuid <<FK>>
    * semester_class_id: uuid <<FK>>
    * payer: uuid <<FK>>
    *status: string
    * price: float8
    * time: timestamp
}


entity "USER_REGISTER_TEACHER_TEACH_CLASS" as user_register_teach_class {
    * id: uuid <<PK>>
    --
    * teacher_id: uuid <<FK>>
    * semester_class_id: uuid <<FK>>
    * time: timestamp
}

' Semester Relationship

user ||..o{ user_register_join_class
user ||..o{ semester_creation: create semester
user_register_teach_class }o..|| user
user_register_teach_class }o..|| semester_class
user_register_join_class }o..|| semester_class
semester_class }o..|| course
semester_class }o..|| semester_creation
holiday }o..|| semester_creation
semester_class ||..o{ schedule
schedule ||..|| lesson_time
@enduml
```


## 8. Class service

```plantuml
@startuml class_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "CLASS" as class {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * user_register_teach_semester: uuid <<FK>>
    * security_code: varchar(255)
    * name: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}

entity "CLASS_HAS_REGISTER_JOIN_SEMESTER_CLASS" as user_join_class {
    * class_id: uuid <<PK,FK>>
    * user_register_join_semester_id: uuid <<PK,FK>>
    --
    * review_star: integer
    * student_feedback: varchar(255)
    * teacher_feedback: varchar(255)
    * final_grade: float
}

entity "USER_REGISTER_JOIN_SEMESTER" as user_register_join_class {
    @ref semester_service
}

entity "TEACHER_REGISTER_TEACH_CLASS" as user_register_teach_class {
    @ref semester_service
}

' Class Relationship

user ||..o{ class: create class
user_join_class }o--|| class
user_join_class ||--|| user_register_join_class
class ||..|| user_register_teach_class: register in
@enduml
```

## 9. Section service

```plantuml
@startuml section_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "COURSE" as course {
    @ref course_service
}

entity "SECTION" as section {
    * id: uuid <<PK>>
    --
    * class_id: uuid <<FK>>
    * name: varchar(255)
    * number: integer
    * teaching_form: boolean
    * recording: text
    * message: text
    * create_time: timestamp
    * update_time: timestamp
}

entity "USER_ATTENDANCE" as user_attendance {
    * id: uuid <<PK>>
    --
    * section_id: uuid <<FK>>
    * student_id: uuid <<FK>>
    * status: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}

entity "SECTION_TEMPLATE" as section_template {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * course_id: uuid <<FK>>
    * name: varchar(255)
    * number: integer
    * teaching_form: boolean
    * create_time: timestamp
    * update_time: timestamp
}


entity "CLASS" as class {
    @ref class_service
}

' Section Relationship
class ||..o{ section
section ||..o{ user_attendance
user_attendance ||..|| user
section_template ||..|| user
section_template ||..|| course
@enduml
```

## 10. User Leave Section
```plantuml
@startuml user_leave_section_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho
entity "SECTION" as section {
    @ref section_service
}

entity "USER" as user {
    @ref user_service
}

entity "CLASS" as class {
    @ref class_service
}

entity "TEACHER_LEAVE" as teacher_leave {
    * id: uuid <<PK>>
    --
    * section_id: uuid <<FK>>
    * class_id: uuid <<FK>>
    * teacher_id: uuid <<FK>>
    * reviewer_id: uuid <<FK>>
    * substitute_teacher_id: uuid <<FK>>
    * description: text
    * status: string
    * create_time: timestamp
    * update_time: timestamp
}

entity "STUDENT_LEAVE" as student_leave {
    * id: uuid <<PK>>
    --
    * section_id: uuid <<FK>>
    * class_id: uuid <<FK>>
    * student_id: uuid <<FK>>
    * reviewer_id: uuid <<FK>>
    * description: text
    * status: string
    * create_time: timestamp
    * update_time: timestamp
}

user ||..o{ teacher_leave : review
user ||..o{ teacher_leave : substitute_teacher
user ||..o{ student_leave : review
teacher_leave }o..|| section
teacher_leave }o..|| user : teacher registration
teacher_leave }o..|| class 
student_leave }o..|| section
student_leave }o..|| user : teacher registration
student_leave }o..|| class
@enduml
```


## 10. Exercise service

```plantuml
@startuml exercise_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "SECTION" as section {
    @ref section_service
}

entity "EXERCISE_LEVEL" as exercise_level {
    * id: uuid <<PK>>
    --
    * name: varchar(255)
    * description: text
    * weight: float8
}

entity "EXERCISE" as exercise {
    * id: uuid <<PK>>
    --
    * section_id: uuid <<FK>>
    * level_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * create_time: timestamp
    * update_time: timestamp
}


' Exercise Relationship
exercise }o..|| exercise_level
exercise }o..|| section
@enduml
```

## 11. Tutorial service

```plantuml
@startuml tutorial_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "SECTION" as section {
    @ref section_service
}

entity "SECTION_TEMPLATE" as section_template {
    @ref section_template_service
}

entity "TUTORIAL" as tutorial {
    * id: uuid <<PK>>
    --
    * section_id: uuid <<FK>>
    * creator_id: uuid <<FK>>
    * name: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}

entity "USER_REGISTER_TUTORIAL" as user_register_tutorial {
    * id: uuid <<PK>>
    --
    * creator_id: uuid <<FK>>
    * section_id: uuid <<FK>>
    * name: varchar(255)
    * status: string
    * create_time: timestamp
    * update_time: timestamp
}


entity "USER_REGISTER_TUTORIAL_PAGE" as user_register_tutorial_page {
    * id: uuid <<PK>>
    --
    * user_register_tutorial_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * number: integer
}

entity "TUTORIAL_TEMPLATE" as tutorial_template {
     * id: uuid <<PK>>
    --
    * section_template_id: uuid <<FK>>
    * name: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}


entity "TUTORIAL_PAGE" as tutorial_page {
    * id: uuid <<PK>>
    --
    * tutorial_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * number: integer
}

entity "TUTORIAL_TEMPLATE_PAGE" as tutorial_template_page {
    * id: uuid <<PK>>
    --
    * tutorial_template_id: uuid <<FK>>
    * name: varchar(255)
    * description: text
    * number: integer
}


' Tutorial Relationship
user ||..o{ tutorial
user ||..o{ tutorial
tutorial ||..o{ tutorial_page
tutorial }o..|| section
tutorial_template ||..|| section_template
tutorial_template ||..o{ tutorial_template_page
user_register_tutorial }o..|| section
user_register_tutorial ||..o{ user_register_tutorial_page
@enduml
```

## 12. Submission service

```plantuml
@startuml submission_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}

entity "EXERCISE_SUBMISSION" as exercise_submission {
    * id: uuid <<PK>>
    --
    * student_id: uuid <<FK>>
    * exercise_id: uuid <<FK>>
    * image_url: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}

entity "CONTEST_SUBMISSION" as contest_submission {
    * id: uuid <<PK>>
    --
    * student_id: uuid <<FK>>
    * contest_id: uuid <<FK>>
    * image_url: varchar(255)
    * create_time: timestamp
    * update_time: timestamp
}

entity "USER_GRADE_EXERCISE_SUBMISSION" as user_grade_exercise_submission {
    * teacher_id: uuid <<PK,FK>>
    * exercise_submission_id: uuid <<PK,FK>>
    --
    * feedback: text
    * score: float8
    * time: timestamp
}

entity "USER_GRADE_CONTEST_SUBMISSION" as user_grade_contest_submission {
    * teacher_id: uuid <<PK,FK>>
    * contest_submission_id: uuid <<PK,FK>>
    --
    * feedback: text
    * score: float8
    * time: timestamp
}

entity "EXERCISE" as exercise {
    @ref exercise_service
}

entity "CONTEST" as contest {
    @ref contest_service
}

' Submission Relationship

exercise_submission }o..|| user
exercise_submission }o..|| exercise
contest_submission }o..|| user
contest_submission }o..|| contest
user_grade_exercise_submission ||--|| user
user_grade_exercise_submission }o--|| exercise_submission
user_grade_contest_submission }o--|| user
user_grade_contest_submission }o--|| contest_submission
@enduml
```

## 13. Notification service

```plantuml
@startuml notification_service
' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "USER" as user {
    @ref user_service
}


entity "NOTIFICATION" as notification {
    * id: uuid <<PK>>
    --
    * name: varchar(255)
    * description: text
    * time: timestamp
}

entity "USER_READ_NOTIFICATION" as user_read_notification {
    * user_id: uuid <<PK,FK>>
    * notification_id: uuid <<PK,FK>>
    --
    * is_read: boolean
}

' Notification Relationship

notification ||--o{ user_read_notification
user_read_notification }o--|| user
@enduml
```
