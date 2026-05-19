 PHP/MySQL platform for student registration, course enrollment, and admin management. Features secure login, real-time seat tracking, and schedule generation.

PHP Code Snippet (Enrollment Handler)
<?php
// enroll.php - Process student enrollment
session_start();
require_once 'db_connect.php';

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $student_id = $_SESSION['user_id'];
    $course_id = mysqli_real_escape_string($conn, $_POST['course_id']);
    
    // Check seat availability
    $check = "SELECT available_seats FROM courses WHERE course_id='$course_id'";
    $result = mysqli_query($conn, $check);
    $row = mysqli_fetch_assoc($result);
    
    if ($row['available_seats'] > 0) {
        $insert = "INSERT INTO enrollments (student_id, course_id, enroll_date) 
                   VALUES ('$student_id', '$course_id', NOW())";
        $update = "UPDATE courses SET available_seats = available_seats - 1 
                   WHERE course_id='$course_id'";
        
        mysqli_query($conn, $insert);
        mysqli_query($conn, $update);
        echo "Enrollment successful!";
    } else {
        echo "Course is full.";
    }
}
?>
