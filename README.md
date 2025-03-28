//front end
//html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health & Legal Services</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Health & Legal Services</h1>
        <nav>
            <ul>
                <li><a href="index.html">Home</a></li>
                <li><a href="register.html">Sign Up</a></li>
                <li><a href="login.html">Login</a></li>
                <li><a href="consultations.html">Consultations</a></li>
                <li><a href="leaderboard.html">Leaderboard</a></li>
                <li><a href="vaccination.html">Vaccination Tracking</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="hero">
            <h2>Empowering Communities</h2>
            <p>Access medical and legal consultations, track vaccinations, and participate in health challenges.</p>
        </section>

        <section id="services">
            <div class="service-card">
                <h3>Video & Audio Consultations</h3>
                <p>Connect with doctors and legal advisors online.</p>
                <a href="consultations.html" class="btn">Learn More</a>
            </div>

            <div class="service-card">
                <h3>Health Challenge Leaderboard</h3>
                <p>Track your progress and compete with others.</p>
                <a href="leaderboard.html" class="btn">View Leaderboard</a>
            </div>

            <div class="service-card">
                <h3>Vaccination Tracking</h3>
                <p>Stay updated with vaccination schedules.</p>
                <a href="vaccination.html" class="btn">Track Now</a>
            </div>
        </section>
    </main>

    <footer>
        <p>&copy; 2025 Health & Legal Services. All rights reserved.</p>
    </footer>

    <script src="script.js"></script>
</body>
</html>

//css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
    text-align: center;
}

header {
    background-color: #2C3E50;
    color: white;
    padding: 15px 0;
}

nav ul {
    list-style: none;
    padding: 0;
}

nav ul li {
    display: inline;
    margin: 0 15px;
}

nav ul li a {
    color: white;
    text-decoration: none;
    font-size: 18px;
}

#hero {
    background-color: #ECF0F1;
    padding: 50px 20px;
}

#services {
    display: flex;
    justify-content: center;
    gap: 20px;
    padding: 20px;
}

.service-card {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    width: 250px;
}

.btn {
    display: inline-block;
    padding: 10px 15px;
    margin-top: 10px;
    background-color: #3498DB;
    color: white;
    text-decoration: none;
    border-radius: 5px;
}

footer {
    background-color: #2C3E50;
    color: white;
    padding: 10px;
    margin-top: 20px;
}
//javascript
document.addEventListener("DOMContentLoaded", function () {
    console.log("JavaScript Loaded");

    const navLinks = document.querySelectorAll("nav ul li a");
    navLinks.forEach(link => {
        link.addEventListener("click", function (event) {
            console.log(`Navigating to: ${event.target.href}`);
        });
    });

    const serviceButtons = document.querySelectorAll(".btn");
    serviceButtons.forEach(button => {
        button.addEventListener("click", function () {
            alert("Feature coming soon!");
        });
    });
});
//Backend code
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.11.5</version>
    </dependency>
</dependencies>
//entity model
package com.healthapp.model;

import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private String password;

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
}
//Data Access Layer
package com.healthapp.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.healthapp.model.User;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
//JWT Token Generator
package com.healthapp.security;

import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import org.springframework.stereotype.Component;
import java.util.Date;

@Component
public class JwtUtil {
    private String secretKey = "your_secret_key";

    public String generateToken(String email) {
        return Jwts.builder()
                .setSubject(email)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10))
                .signWith(SignatureAlgorithm.HS256, secretKey)
                .compact();
    }
}
//Service Layer
package com.healthapp.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;
import com.healthapp.model.User;
import com.healthapp.repository.UserRepository;
import com.healthapp.security.JwtUtil;
import java.util.Optional;

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    @Autowired
    private JwtUtil jwtUtil;

    private BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();

    public ResponseEntity<String> register(User user) {
        user.setPassword(passwordEncoder.encode(user.getPassword()));
        userRepository.save(user);
        return ResponseEntity.ok("User registered successfully!");
    }

    public ResponseEntity<String> login(User user) {
        Optional<User> existingUser = userRepository.findByEmail(user.getEmail());
        if (existingUser.isPresent() && passwordEncoder.matches(user.getPassword(), existingUser.get().getPassword())) {
            String token = jwtUtil.generateToken(user.getEmail());
            return ResponseEntity.ok(token);
        }
        return ResponseEntity.status(401).body("Invalid credentials");
    }
}
//REST API Controller
package com.healthapp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import com.healthapp.model.User;
import com.healthapp.service.UserService;

@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;

    @PostMapping("/register")
    public ResponseEntity<String> register(@RequestBody User user) {
        return userService.register(user);
    }

    @PostMapping("/login")
    public ResponseEntity<String> login(@RequestBody User user) {
        return userService.login(user);
    }
}
//Consultations, Leaderboard, Vaccination API
package com.healthapp.controller;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/services")
public class HealthController {

    @GetMapping("/consultations")
    public String getConsultations() {
        return "List of available consultations";
    }

    @GetMapping("/leaderboard")
    public String getLeaderboard() {
        return "Health challenge leaderboard";
    }

    @GetMapping("/vaccination")
    public String getVaccinationTracking() {
        return "Vaccination tracking details";
    }
}
//Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/healthapp
spring.datasource.username=root
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8080
//Main Entry Point
package com.healthapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HealthAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(HealthAppApplication.class, args);
    }
}
//sql command
CREATE DATABASE healthapp;
USE healthapp;
//run the application with 
mvn spring-boot:run

