# EXP_03_-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration

## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM:
1. Create Spring Boot Project
2. Add dependencies:
   - Spring Web
   - Spring Data JPA
   - H2 Database or MySQL
   - Spring Boot DevTools
3. Configure application.properties
4. Define database connection
5. Enable Hibernate auto DDL
6. Create Student Entity Class
7. Annotate with @Entity
8. Define fields with @Id, @GeneratedValue, etc.
9. Create StudentRepository
10. Extend JpaRepository<Student, Long> for CRUD methods
11. Create StudentController
12. Handle HTTP methods:
    - POST /students → Add student
    - GET /students → Get all students
    - GET /students/{id} → Get student by ID
    - PUT /students/{id} → Update student
    - DELETE /students/{id} → Delete student

## PROGRAM CODE

### pom.xml
```xml
<dependencies>
    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- H2 Database (In-memory) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```
### application.properties
```
spring.datasource.url=jdbc:h2:mem:studentdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=user
spring.datasource.password=1234
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```
### Student.java
```java
package com.example.sample1.Entity;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class Student {
    @Id
    private int id;
    private String name;
    private String dept;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDept() {
        return dept;
    }

    public void setDept(String dept) {
        this.dept = dept;
    }
}

```
### StudentRepository.java
```java
package com.example.sample1.Repository;

import com.example.sample1.Entity.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Integer> {

}

```
### StudentController.java
```java
package com.example.sample1.Controller;

import com.example.sample1.Entity.Student;
import com.example.sample1.Service.StudentService;

import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/student")
public class StudentController {
    StudentService studentService;

    StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    @PostMapping("/add")
    public Student addStudent(@RequestBody Student student) {
        return studentService.addStudent(student);
    }

    @GetMapping("/get")
    public List<Student> getStudents() {
        return studentService.getStudents();
    }

    @PutMapping("/update")
    public Student updateStudent(@RequestBody Student student) {
        return studentService.updateStudent(student);
    }

    @DeleteMapping("delete")
    public String deleteStudent(@RequestBody Student student) {
        return studentService.deleteStuent(student);
    }
}

```
### StudentService.java
```java
package com.example.sample1.Service;

import com.example.sample1.Entity.Student;
import com.example.sample1.Repository.StudentRepository;

import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;

import java.util.List;

@Service
public class StudentService {
    StudentRepository studentRepository;

    StudentService(StudentRepository studentRepository) {
        this.studentRepository = studentRepository;
    }

    public Student addStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    public List<Student> getStudents() {
        return studentRepository.findAll();
    }

    public Student updateStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    public Student updateStudent(@PathVariable int id, @RequestBody Student student) {
        Student data = studentRepository.findById(id).get();
        data.setName(student.getName());
        data.setId(student.getId());
        data.setDept(student.getDept());
        return studentRepository.save(data);
    }

    public String deleteStuent(@RequestBody Student student) {
        studentRepository.delete(student);
        return "Student Deleted Successfully";
    }
}

```
### Sample1Application.java
```java
package com.example.sample1;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Sample1Application {
	public static void main(String[] args) {
		SpringApplication.run(Sample1Application.class, args);
	}

}

```
### Output
#### Create
<img width="959" height="428" alt="image" src="https://github.com/user-attachments/assets/11fc3cbd-5c09-4ce6-bab1-9d038750c7bf" />

#### Read
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/1707852a-2d65-4930-b967-6d9f3d33aef4" />

#### Update
<img width="949" height="506" alt="image" src="https://github.com/user-attachments/assets/6aed40d7-1ae7-4454-9759-0e0e552a87d1" />

#### Delete
<img width="959" height="503" alt="image" src="https://github.com/user-attachments/assets/1a4c4713-1dbb-4770-bb06-b477f64a2e60" />

#### H2 Database
<img width="957" height="478" alt="image" src="https://github.com/user-attachments/assets/75be79e8-83a4-4a2a-b8fb-22c003a8267c" />

### Result
Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate) is successfully executed.



