# Usecase diagrams

## 0. User Hierarchy

```plantuml
@startuml user_hierarchy
actor "User" as u
actor "Registered User" as ru
actor "New User" as nu
actor "Student" as student
actor "Student(not Child)" as parent
actor "Student(Child)" as child
actor "Teacher" as teacher
actor "Admin" as admin

u <|-- ru
u <|-- nu
ru <|-- student
ru <|-- teacher
ru <|-- admin
student <|-- parent
student <|-- child
@enduml
```

## 1. User service

```plantuml
@startuml user_service
left to right direction
actor "Registered User" as ru
actor "New User" as nu
actor "Admin" as admin

package "User Service" <<Subsystem>> {
    usecase "Register Student" as register
    usecase "Update User" as update
    usecase "View Users" as view
    usecase "Search Users" as search
    usecase "Delete User" as delete
    usecase "Restore User" as restore
    usecase "View User Detail" as detail
    usecase "View Self Detail" as self_detail
    usecase "Authenticate" as auth
}

admin -|> ru

self_detail -- ru
register -- nu
update -- ru
view -- admin
search ..> view: <<extend>>
detail ..> view: <<extend>>
delete ..> view: <<extend>>

restore ..> delete: <<extend>>

auth <.. self_detail: <<include>>
auth <.. update: <<include>>
auth <.. detail: <<include>>
auth <.. view: <<include>>
auth <.. search: <<include>>
auth <.. delete: <<include>>
auth <.r. restore: <<include>>

@enduml
```

## 2. Teacher service

```plantuml
@startuml teacher_service
left to right direction
actor "Admin" as admin

package "Teacher Service" <<Subsystem>> {
    usecase "Create Teachers" as create
    usecase "View Teachers" as view
    usecase "Search Teachers" as search
    usecase "Delete Teacher" as delete
    usecase "Restore Teacher" as restore
    usecase "View Detail" as detail
    usecase "Authenticate" as auth
}

create -- admin
view -- admin
search ..> view: <<extend>>
detail ..> view: <<extend>>
delete ..> view: <<extend>>
restore ..> delete: <<extend>>

auth <.. create: <<include>>
auth <.. detail: <<include>>
auth <.. view: <<include>>
auth <.. search: <<include>>
auth <.. delete: <<include>>
auth <. restore: <<include>>
@enduml
```


## 3. Children service

```plantuml
@startuml children_service
left to right direction
actor "Student(not Child)" as student

package "Children Service" <<Subsystem>> {
    usecase "Create Child" as create
    usecase "View Children" as view
    usecase "Search Children" as search
    usecase "Delete Child" as delete
    usecase "Restore Child" as restore
    usecase "View Detail" as detail
    usecase "Authenticate" as auth
}

create -- student
view -- student
search ..> view: <<extend>>
detail ..> view: <<extend>>
delete ..> view: <<extend>>
restore ..> delete: <<extend>>

auth <.. create: <<include>>
auth <.. detail: <<include>>
auth <.. view: <<include>>
auth <.. search: <<include>>
auth <.. delete: <<include>>
auth <.r. restore: <<include>>
@enduml
```

## 5. Art service

```plantuml
@startuml art_service
left to right direction
actor "User" as u
actor "Admin" as sa

package "Art Service" <<Subsystem>> {
    usecase "Create Art Level" as create_level
    usecase "View Art Levels" as view_level
    usecase "Search Art Levels" as search_level
    usecase "Update Art Level" as update_level
    usecase "Delete Art Level" as delete_level
    usecase "Create Art Type" as create_type
    usecase "View Art Types" as view_type
    usecase "Search Art Types" as search_type
    usecase "Update Art Type" as update_type
    usecase "Delete Art Type" as delete_type
    usecase "Authenticate" as auth
}

sa --|> u

view_level -- u
view_type -- u
create_level -- sa
update_level -- sa
delete_level -- sa
create_type -- sa
update_type -- sa
delete_type -- sa
search_level ..> view_level: <<extend>>
search_type ..> view_type: <<extend>>
update_level ..> view_level: <<include>>
delete_level ..> view_level: <<include>>
update_type ..> view_type: <<include>>
delete_type ..> view_type: <<include>>

auth <.. create_level: <<include>>
auth <.. update_level: <<include>>
auth <.. delete_level: <<include>>
auth <.. create_type: <<include>>
auth <.. update_type: <<include>>
auth <.. delete_type: <<include>>
@enduml
```

## 6. Contest service

```plantuml
@startuml contest_service
left to right direction
actor "User" as u
actor "Admin" as admin

package "Contest Service" <<Subsystem>> {
    usecase "Create Contests" as create
    usecase "View Contests" as view
    usecase "Search Contests" as search
    usecase "Delete Contest" as delete
    usecase "Update Contest" as update
    usecase "View Detail Contest" as detail
    usecase "Authenticate" as auth
}

admin --|> u

view -- u
create -- admin
delete -- admin
update -- admin
search ..> view: <<extend>>
detail ..> view: <<extend>>
delete ..> view: <<include>>
update ..> view: <<include>>

auth <.. create: <<include>>
auth <.. update: <<include>>
auth <.. view: <<include>>
auth <.. search: <<include>>
auth <.. delete: <<include>>
@enduml
```

## 7. Course service

```plantuml
@startuml course_service
left to right direction
actor "User" as u
actor "Teacher" as teacher
actor "Admin" as admin

package "Course Service" <<Subsystem>> {
    usecase "Create Courses" as create
    usecase "View Courses" as view
    usecase "Update Courses" as update
    usecase "Search Courses" as search
    usecase "Delete Course" as delete
    usecase "Register Teach Course" as register_teach
    usecase "Review Register Teach Course" as review_regiter_teach
    usecase "View Detail Course" as detail
    usecase "Authenticate" as auth
}

teacher --|> u
admin --|> u

view -- u
create -- admin
delete -- admin
update -- admin
review_regiter_teach -- admin
register_teach -- teacher
register_teach .r.> view: <<include>>
delete ..> view: <<include>>
update ..> view: <<include>>
search ..> view: <<extend>>
detail ..> view: <<extend>>
review_regiter_teach ..> register_teach: <<include>>

auth <.. create: <<include>>
auth <.. update: <<include>>
auth <.. delete: <<include>>
auth <.. register_teach: <<include>>
auth <.. review_regiter_teach: <<include>>
@enduml
```

## 8. Schedule service

```plantuml
@startuml schedule_service
left to right direction
actor "User" as u
actor "Admin" as admin

package "Schedule Service" <<Subsystem>> {
    usecase "Create Schedule" as create_schedule
    usecase "View Schedules" as view_schedule
    usecase "Search Schedules" as search_schedule
    usecase "Update Schedule" as update_schedule
    usecase "Delete Schedule" as delete_schedule
    usecase "View Detail Schedule" as detail_schedule
    usecase "Create Lesson Time" as create_time
    usecase "View Lesson Times" as view_time
    usecase "Search Lesson Times" as search_time
    usecase "Update Lesson Time" as update_time
    usecase "Delete Lesson Time" as delete_time
    usecase "View Detail Lesson Time" as detail_time
    usecase "Authenticate" as auth
}

admin --|> u

view_time -- u
view_schedule -- u
create_time -- admin
update_time -- admin
delete_time -- admin
create_schedule -- admin
update_schedule -- admin
delete_schedule -- admin
search_time ..> view_time: <<extend>>
detail_time ..> view_time: <<extend>>
delete_time ..> view_time: <<include>>
update_time ..> view_time: <<include>>
search_schedule ..> view_schedule: <<extend>>
detail_schedule ..> view_schedule: <<extend>>
delete_schedule ..> view_schedule: <<include>>
update_schedule ..> view_schedule: <<include>>

auth <.. create_time: <<include>>
auth <.. update_time: <<include>>
auth <.. delete_time: <<include>>
auth <.. create_schedule: <<include>>
auth <.. update_schedule: <<include>>
auth <.. delete_schedule: <<include>>
@enduml
```

## 9. Semester service

```plantuml
@startuml semester_service
left to right direction
actor "User" as u
actor "Admin" as admin
actor "Teacher" as teacher
actor "Student" as student

package "Semester Service" <<Subsystem>> {
    usecase "Create Semester Course" as create_course
    usecase "View Semester Courses" as view_course
    usecase "Search Semester Courses" as search_course
    usecase "Update Semester Course" as update_course
    usecase "Delete Semester Course" as delete_course
    usecase "View Detail Semester Course" as detail_course
    usecase "Create Semester" as create_semester
    usecase "View Semesters" as view_semester
    usecase "Search Semesters" as search_semester
    usecase "Update Semester" as update_semester
    usecase "Delete Semester" as delete_semester
    usecase "View Detail Semester" as detail_semester
    usecase "Review Semester Creation" as review_semester
    usecase "Register Teach Semester" as register_teach_semester
    usecase "Register Join Semester" as register_join_semester
    usecase "Authenticate" as auth
}

admin --|> u
teacher --|> u
student --|> u

view_semester -- u
view_course -- u
create_semester -- admin
update_semester -- admin
delete_semester -- admin
create_course -- admin
update_course -- admin
delete_course -- admin
register_teach_semester -- teacher
register_join_semester -- student
review_semester -- admin
register_teach_semester ..> create_course: <<include>>
register_join_semester ..> create_course: <<include>>
review_semester ..> create_semester: <<include>>
search_semester ..> view_semester: <<extend>>
detail_semester ..> view_semester: <<extend>>
delete_semester ..> view_semester: <<include>>
update_semester ..> view_semester: <<include>>
search_course ..> view_course: <<extend>>
detail_course ..> view_course: <<extend>>
delete_course ..> view_course: <<include>>
update_course ..> view_course: <<include>>

auth <. register_teach_semester: <<include>>
auth <. register_join_semester: <<include>>
auth <.. review_semester: <<include>>
auth <.. create_semester: <<include>>
auth <.. update_semester: <<include>>
auth <.. delete_semester: <<include>>
auth <.. create_course: <<include>>
auth <.. update_course: <<include>>
auth <.. delete_course: <<include>>
@enduml
```

## 10. Class service

```plantuml
@startuml class_service
left to right direction
actor "Registered User" as ru
actor "Admin" as admin

package "Class Service" <<Subsystem>> {
    usecase "Create Class" as create_class
    usecase "View Classes" as view_class
    usecase "Search Classes" as search_class
    usecase "Update Class" as update_class
    usecase "Delete Class" as delete_class
    usecase "View Detail Class" as detail_class
    usecase "Review Semester Creation" <<@ref semester_service>> as review_semester
    usecase "Register Teach Semester" <<@ref semester_service>> as register_teach_semester
    usecase "Register Join Semester" <<@ref semester_service>> as register_join_semester
    usecase "Authenticate" as auth
}

admin --|> ru

view_class -- ru
create_class -- admin
update_class -- admin
delete_class -- admin
create_class ..> register_teach_semester: <<include>>
create_class ..> register_join_semester: <<include>>
create_class ..> review_semester: <<include>>
search_class ..> view_class: <<extend>>
detail_class ..> view_class: <<extend>>
update_class ..> view_class: <<include>>
delete_class ..> view_class: <<include>>

auth <.. create_class: <<include>>
auth <.. update_class: <<include>>
auth <.. delete_class: <<include>>
@enduml
```

## 11. Section service

```plantuml
@startuml section_service
left to right direction
actor "Registered User" as ru
actor "Admin" as admin
actor "Teacher" as teacher

package "Section Service" <<Subsystem>> {
    usecase "Create Section" as create_section
    usecase "Create Make up Section" as create_makeup_section
    usecase "View Sections" as view_section
    usecase "Search Sections" as search_section
    usecase "Update Section" as update_section
    usecase "Delete Section" as delete_section
    usecase "View Detail Section" as detail_section
    usecase "View Classes" <<@ref class_service>> as view_class
    usecase "Authenticate" as auth
}

admin --|> ru
teacher --|> ru

view_section -- ru
create_section -- admin
create_makeup_section -- teacher
update_section -- teacher
delete_section -- teacher
view_template -- teacher
view_section .> view_class: <<include>>
create_makeup_section ..> view_section: <<extend>>
search_section ..> view_section: <<extend>>
detail_section ..> view_section: <<extend>>
update_section ..> view_section: <<include>>
delete_section ..> view_section: <<include>>

auth <.. create_makeup_section: <<include>>
auth <.. create_section: <<include>>
auth <.. update_section: <<include>>
auth <.. delete_section: <<include>>
@enduml
```
## 12. Blog service
```plantuml
@startuml blog_service
left to right direction
actor "User" as u
actor "Admin" as admin

package "Blog Service" <<Subsystem>> {
    usecase "Create Blogs" as create
    usecase "View Blogs" as view
    usecase "Search Blogs" as search
    usecase "Delete Blog" as delete
    usecase "Update Blog" as update
    usecase "Create Tags" as create_tag
    usecase "View Tags" as view_tag
    usecase "Search Tags" as search_tag
    usecase "Delete Tags" as delete_tag
    usecase "Update Tags" as update_tag
    usecase "View Detail Blog" as detail
    usecase "Authenticate" as auth
}

admin --|> u

view -- u
create -- admin
delete -- admin
update -- admin
create_tag -- admin
delete_tag -- admin
update_tag -- admin
view_tag -- admin

search ..> view: <<extend>>
detail ..> view: <<extend>>
search_tag ..> view_tag: <<extend>>

delete ..> view: <<include>>
update ..> view: <<include>>

auth <.. create: <<include>>
auth <.. update: <<include>>
auth <.. delete: <<include>>
auth <.. create_tag: <<include>>
auth <.. update_tag: <<include>>
auth <.. delete_tag: <<include>>
@enduml
```

## 13. Exercise service
```plantuml
@startuml exercise_service
left to right direction
actor "Registered User" as ru
actor "Teacher" as teacher

package "Exercise Service" <<Subsystem>> {
    usecase "Create Exercise" as create_exercise
    usecase "View Exercises" as view_exercise
    usecase "Search Exercises" as search_exercise
    usecase "Update Exercise" as update_exercise
    usecase "Delete Exercise" as delete_exercise
    usecase "View Detail Exercise" as detail_exercise
    usecase "View Classes" <<@ref class_service>> as view_class
    usecase "Authenticate" as auth
}



view_exercise -- ru
view_exercise -- teacher
create_exercise -- teacher
update_exercise -- teacher
delete_exercise -- teacher
view_exercise .> view_class: <<include>>

search_exercise ..> view_exercise: <<extend>>
detail_exercise ..> view_exercise: <<extend>>


auth <.. create_exercise: <<include>>
auth <.. update_exercise: <<include>>
auth <.. delete_exercise: <<include>>
@enduml
```

## 15. Tutorial service
```plantuml
@startuml tutorial_service
left to right direction
actor "Registered User" as ru
actor "Teacher" as teacher
actor "Admin" as admin

package "Tutorial Service" <<Subsystem>> {
    usecase "Create Tutorial" as create_tutorial
    usecase "View Tutorial" as view_tutorial
    usecase "Update Tutorial" as update_tutorial
    usecase "Delete Tutorial" as delete_tutorial
    usecase "View Detail Tutorial" as detail_tutorial
    usecase "View Classes" <<@ref class_service>> as view_class
    usecase "Authenticate" as auth
    usecase "Review Tutorial" as review_tutorial
}



view_tutorial -- ru
review_tutorial -- admin
review_tutorial .> create_tutorial: <<include>>
view_tutorial -- teacher
update_tutorial -- teacher
delete_tutorial -- teacher
view_tutorial .> view_class: <<include>>
detail_tutorial ..> view_tutorial: <<extend>>


auth <.. create_tutorial: <<include>>
auth <.. update_tutorial: <<include>>
auth <.. delete_tutorial: <<include>>
auth <.. review_tutorial: <<include>>
@enduml
```

## 16. Submission service

```plantuml
@startuml submission_service
left to right direction
actor "Registered User" as ru
actor "Teacher" as teacher

package "Submission Service" <<Subsystem>> {
    usecase "Create Submission" as create_submission
    usecase "View Submission" as view_submission
    usecase "Search Submission" as search_submission
    usecase "Update Submission" as update_submission
    usecase "Delete Submission" as delete_submission
    usecase "View Detail Submission" as detail_submission
    usecase "Create Grade Submission" as create_grade_submission
    usecase "View Grade Submission" as view_grade_submission
    usecase "Update Grade Submission" as update_grade_submission
    usecase "Delete Grade Submission" as delete_grade_submission
    usecase "View Grade Detail Submission" as detail_grade_submission
    usecase "View Classes" <<@ref class_service>> as view_class
    usecase "View Contests" <<@ref contest_service>> as view_contest
    usecase "Authenticate" as auth
}



view_submission -- ru
create_submission -- ru
update_submission -- ru
delete_submission -- ru
view_grade_submission -- ru
view_submission -- teacher
create_grade_submission -- teacher
view_grade_submission -- teacher
update_grade_submission -- teacher
delete_grade_submission -- teacher
view_submission .> view_class: <<include>>
view_submission .> view_contest: <<include>>

search_submission ..> view_submission: <<extend>>
detail_submission ..> view_submission: <<extend>>

detail_grade_submission ..> view_grade_submission: <<extend>>


auth <.. create_submission: <<include>>
auth <.. update_submission: <<include>>
auth <.. delete_submission: <<include>>
auth <.. create_grade_submission: <<include>>
auth <.. update_grade_submission: <<include>>
auth <.. delete_grade_submission: <<include>>
@enduml
```

## 17. Notification service

```plantuml
@startuml notification_service
left to right direction
actor "User" as user
actor "Registered User" as ru
actor "Admin" as admin
actor "Teacher" as teacher

package "Notification Service" <<Subsystem>> {
    usecase "Create Notification Anonymous" as create_notification_anonymous
    usecase "View Notification Anonymous" as view_notification_anonymous
    usecase "Update Notification Anonymous" as update_notification_anonymous
    usecase "Delete Notification Anonymous" as delete_notification_anonymous
    usecase "View Detail Notification Anonymous" as detail_notification_anonymous
    usecase "Create Notification Class" as create_notification_class
    usecase "View Notification Class" as view_notification_class
    usecase "Update Notification Class" as update_notification_class
    usecase "Delete Notification Class" as delete_notification_class
    usecase "View Detail Notification Class" as detail_notification_class
    usecase "View Classes" <<@ref class_service>> as view_class
    usecase "Authenticate" as auth
}

ru --|> user
teacher --|> user
admin --|> user


view_notification_class -- ru
create_notification_class -- teacher
update_notification_class -- teacher
delete_notification_class -- teacher
view_notification_class .> view_class: <<include>>



view_notification_anonymous -- user
create_notification_anonymous -- admin
update_notification_anonymous -- admin
delete_notification_anonymous -- admin

detail_notification_class ..> view_notification_class: <<extend>>
detail_notification_anonymous ..> view_notification_anonymous: <<extend>>


auth <.. create_notification_class: <<include>>
auth <.. update_notification_class: <<include>>
auth <.. delete_notification_class: <<include>>

auth <.. create_notification_anonymous: <<include>>
auth <.. update_notification_anonymous: <<include>>
auth <.. delete_notification_anonymous: <<include>>
@enduml
```
