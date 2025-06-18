# CourseCraft
package com.courseapplication.courseapplication;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CourseapplicationApplication {

	public static void main(String[] args) {
		SpringApplication.run(CourseapplicationApplication.class, args);
	}

}
////////////////////////////////////////////////////////////

Mycontroller.java

package com.courseapplication.courseapplication.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import com.courseapplication.courseapplication.entities.Course;
import com.courseapplication.courseapplication.services.CourseService;

import java.util.List;
@RestController

@CrossOrigin(origins = "http://localhost:3000") 
public class MyController {
	@Autowired
	
	private CourseService courseService;
    @GetMapping("/")
    public String home() {
        return "Welcome home!";
    }
	@GetMapping("/home")
	public String homePage()
	{
		return "Welcome to  courses application";
	}
	//get the courses
	@GetMapping("/courses")
	public List<Course> getCourses()
	{
		return this.courseService.getCourses();
	}
	@GetMapping("/courses/{courseId}")
	public Course getCourse(@PathVariable String courseId)
	{
		return this.courseService.getCourse(Long.parseLong(courseId));
	}
	@PostMapping("/courses")
	public Course addCourse(@RequestBody Course course) 
	{
		return this.courseService.addCourse(course);
	}
	@PutMapping("/courses")
	public Course updateCourse(@RequestBody Course course)
	{
		return this.courseService.updateCourse(course);
	}
	@DeleteMapping("/courses/{courseId}")
	public ResponseEntity<HttpStatus> deleteCourse(@PathVariable String courseId)
	{
		try
		{
			this.courseService.deleteCourse(Long.parseLong(courseId));
			return new ResponseEntity<>(HttpStatus.OK);
		} catch (Exception e )
		{
			return new ResponseEntity <>(HttpStatus.INTERNAL_SERVER_ERROR);
		}
	}
}
///////////////////////////////////////////
package com.courseapplication.courseapplication.Dao;
import org.springframework.data.jpa.repository.JpaRepository;
import com.courseapplication.courseapplication.entities.Course;
public interface CourseDao extends JpaRepository<Course,Long>{

}
/////////////////////////////////////////
package com.courseapplication.courseapplication.entities;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity

public class Course {
	@Id
	private long id;
	private String title;
	private String description;
	public Course() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Course(long id, String title, String description) {
		super();
		this.id = id;
		this.title = title;
		this.description = description;
	}
	public long getId() {
		return id;
	}
	public void setId(long id) {
		this.id = id;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getDescription() {
		return description;
	}
	public void setDescription(String description) {
		this.description = description;
	}
	@Override
	public String toString() {
		return "Course [id=" + id + ", title=" + title + ", description=" + description + "]";
	}
}
//////////////////////////////////////
package com.courseapplication.courseapplication.services;
import com.courseapplication.courseapplication.entities.Course;
import java.util.List;
public interface CourseService {
public List<Course> getCourses();
public Course getCourse(long courseId);
public  Course addCourse(Course course);
public Course updateCourse(Course course);
public void deleteCourse(long long1);
}
////////////////////////////////////////////
package com.courseapplication.courseapplication.services;
import com.courseapplication.courseapplication.Dao.CourseDao;
import com.courseapplication.courseapplication.entities.Course;
//import org.springframework.stereotype.Service;
import java.util.List;
import java.util.Optional;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.RequestBody;
@Service
public class CourseServiceImpl implements CourseService {
	@Autowired
	
	private CourseDao courseDao;
	//List<Course> list;
	public  CourseServiceImpl() {
		
	//list=new ArrayList<>();
	//list.add(new Course(145,"Java Core Course","this course contain basic of java"));
	//list.add(new Course(4343,"spring boot course","creating rest api using spring boot"));
	}
	@Override
	public List<Course> getCourses() {
	return courseDao.findAll();
	}
	@Override
	public Course getCourse (long courseId)
	{
		//Course c=null;
		//for(Course course:list)
		//{
			//if(course.getId()==courseId)
			//{
				//c=course;
				//break;
		//	}
		//}
		 return courseDao.findById(courseId).orElse(null);
	}
	@Override
	public Course addCourse( @RequestBody Course course) {
		return courseDao.save(course);
		
	}
	@Override
	public Course updateCourse(Course course) {
		//list.add(course);
		courseDao.save(course);
		return course;
	}
	
	
	@Override
	public void deleteCourse(long parseLong)
	{
		  Optional<Course> optionalCourse = courseDao.findById(parseLong);
		    if (optionalCourse.isPresent()) {
		        courseDao.delete(optionalCourse.get());
		    } else {
		        // Handle the case where course is not found, e.g., throw custom exception or log
		        System.out.println("Course not found with ID: " + parseLong);
		    }
	}
}
	
///////////////////////////////////////////
package com.courseapplication.courseapplication;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class CourseapplicationApplicationTests {

	@Test
	void contextLoads() {
	}

}

	

