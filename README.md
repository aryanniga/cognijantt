

/**
 * CognizantApplicationDemo.java
 * A comprehensive Java application demonstrating core competencies for a Java Developer role
 */

 import java.util.*;
 import java.io.*;
 import java.nio.file.*;
 import java.time.LocalDateTime;
 import java.time.format.DateTimeFormatter;
 import java.util.concurrent.*;
 import java.util.stream.Collectors;
 
 public class CognizantApplicationDemo {
     
     private static final Logger logger = Logger.getLogger(CognizantApplicationDemo.class.getName());
     
     public static void main(String[] args) {
         System.out.println("Welcome to the Cognizant Application Demo");
         System.out.println("------------------------------------------");
         
         // Create a user profile
         UserProfile userProfile = new UserProfile.Builder()
                 .withName("Your Name")
                 .withEmail("your.email@example.com")
                 .withSkills(Arrays.asList("Java", "Spring Boot", "Microservices", "REST API", "SQL"))
                 .withExperience(5) // years
                 .build();
         
         // Display user data
         System.out.println("\nCandidate Profile:");
         System.out.println(userProfile);
         
         // Demonstrate collection handling with streams
         System.out.println("\nSkill Analysis:");
         Map<String, Integer> skillProficiency = createSkillProficiencyMap();
         analyzeSkills(skillProficiency);
         
         // Demonstrate multithreading
         System.out.println("\nRunning concurrent tasks:");
         runConcurrentTasks();
         
         // Demonstrate file handling
         System.out.println("\nFile operations:");
         try {
             handleFileOperations();
         } catch (IOException e) {
             logger.log(Level.SEVERE, "Error during file operations", e);
             System.out.println("Error during file operations: " + e.getMessage());
         }
         
         // Demonstrate REST API understanding with mock client
         System.out.println("\nREST API Client Simulation:");
         simulateRestApiClient();
         
         // Demonstrate exception handling
         System.out.println("\nException handling:");
         try {
             demonstrateExceptionHandling();
         } catch (CustomApplicationException e) {
             System.out.println("Properly caught custom exception: " + e.getMessage());
         }
         
         System.out.println("\nThank you for reviewing my application demo!");
     }
     
     // Builder pattern demonstration with UserProfile
     static class UserProfile {
         private final String name;
         private final String email;
         private final List<String> skills;
         private final int yearsOfExperience;
         
         private UserProfile(Builder builder) {
             this.name = builder.name;
             this.email = builder.email;
             this.skills = builder.skills;
             this.yearsOfExperience = builder.yearsOfExperience;
         }
         
         @Override
         public String toString() {
             return "Name: " + name + "\n" +
                    "Email: " + email + "\n" +
                    "Skills: " + String.join(", ", skills) + "\n" +
                    "Experience: " + yearsOfExperience + " years";
         }
         
         static class Builder {
             private String name;
             private String email;
             private List<String> skills = new ArrayList<>();
             private int yearsOfExperience;
             
             public Builder withName(String name) {
                 this.name = name;
                 return this;
             }
             
             public Builder withEmail(String email) {
                 this.email = email;
                 return this;
             }
             
             public Builder withSkills(List<String> skills) {
                 this.skills = new ArrayList<>(skills);
                 return this;
             }
             
             public Builder withExperience(int years) {
                 this.yearsOfExperience = years;
                 return this;
             }
             
             public UserProfile build() {
                 // Add validation logic if needed
                 if (name == null || name.isEmpty()) {
                     throw new IllegalStateException("Name cannot be empty");
                 }
                 
                 return new UserProfile(this);
             }
         }
     }
     
     // Map creation for skill proficiency
     private static Map<String, Integer> createSkillProficiencyMap() {
         Map<String, Integer> skillMap = new HashMap<>();
         skillMap.put("Java", 9);
         skillMap.put("Spring Boot", 8);
         skillMap.put("Microservices", 8);
         skillMap.put("REST API", 9);
         skillMap.put("SQL", 8);
         skillMap.put("JUnit", 7);
         skillMap.put("Docker", 7);
         skillMap.put("Git", 9);
         return skillMap;
     }
     
     // Demonstrate Stream API usage
     private static void analyzeSkills(Map<String, Integer> skillProficiency) {
         // Find top skills
         System.out.println("Top skills (rated 8+):");
         skillProficiency.entrySet().stream()
                 .filter(entry -> entry.getValue() >= 8)
                 .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
                 .forEach(entry -> System.out.println("  - " + entry.getKey() + ": " + entry.getValue() + "/10"));
         
         // Calculate average proficiency
         double averageProficiency = skillProficiency.values().stream()
                 .mapToInt(Integer::intValue)
                 .average()
                 .orElse(0.0);
         
         System.out.printf("Average skill proficiency: %.1f/10\n", averageProficiency);
     }
     
     // Demonstrate multithreading and concurrency
     private static void runConcurrentTasks() {
         ExecutorService executor = Executors.newFixedThreadPool(3);
         
         // Create tasks
         Callable<String> task1 = () -> {
             Thread.sleep(1000); // Simulate work
             return "Database connection pool initialized";
         };
         
         Callable<String> task2 = () -> {
             Thread.sleep(800); // Simulate work
             return "User authentication service started";
         };
         
         Callable<String> task3 = () -> {
             Thread.sleep(1200); // Simulate work
             return "API endpoints configured";
         };
         
         List<Callable<String>> tasks = Arrays.asList(task1, task2, task3);
         
         try {
             List<Future<String>> results = executor.invokeAll(tasks);
             
             for (Future<String> result : results) {
                 System.out.println("  - " + result.get());
             }
             
             System.out.println("All concurrent tasks completed successfully.");
         } catch (InterruptedException | ExecutionException e) {
             Thread.currentThread().interrupt();
             System.out.println("Concurrent execution interrupted: " + e.getMessage());
         } finally {
             executor.shutdown();
         }
     }
     
     // Demonstrate file handling
     private static void handleFileOperations() throws IOException {
         String fileName = "application_data.txt";
         
         // Write to file
         List<String> lines = Arrays.asList(
             "Application Demo Data",
             "Generated on: " + LocalDateTime.now().format(DateTimeFormatter.ISO_LOCAL_DATE_TIME),
             "Java Version: " + System.getProperty("java.version"),
             "Candidate applying for Java Developer position at Cognizant"
         );
         
         Files.write(Paths.get(fileName), lines, StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING);
         System.out.println("File created: " + fileName);
         
         // Read from file
         System.out.println("Reading file content:");
         Files.lines(Paths.get(fileName)).forEach(line -> System.out.println("  " + line));
         
         // Cleanup
         Files.delete(Paths.get(fileName));
         System.out.println("File deleted after operation");
     }
     
     // Mock REST API client to demonstrate understanding
     private static void simulateRestApiClient() {
         RestApiClient client = new RestApiClient("https://api.cognizant.example.com");
         
         // GET request simulation
         try {
             ApiResponse response = client.get("/api/jobs/java-developer");
             System.out.println("GET Response: Status " + response.getStatusCode());
             System.out.println("  Job Positions Available: " + response.getData().get("positions"));
         } catch (ApiException e) {
             System.out.println("API Error: " + e.getMessage());
         }
         
         // POST request simulation for job application
         Map<String, Object> applicationData = new HashMap<>();
         applicationData.put("position", "Senior Java Developer");
         applicationData.put("name", "Your Name");
         applicationData.put("email", "your.email@example.com");
         applicationData.put("yearsOfExperience", 5);
         
         try {
             ApiResponse response = client.post("/api/applications/submit", applicationData);
             System.out.println("POST Response: Status " + response.getStatusCode());
             System.out.println("  Application ID: " + response.getData().get("applicationId"));
             System.out.println("  Submission Time: " + response.getData().get("submissionTime"));
         } catch (ApiException e) {
             System.out.println("API Error: " + e.getMessage());
         }
     }
     
     // Mock REST API Client class
     static class RestApiClient {
         private final String baseUrl;
         
         public RestApiClient(String baseUrl) {
             this.baseUrl = baseUrl;
         }
         
         public ApiResponse get(String endpoint) throws ApiException {
             // Simulate HTTP GET request
             System.out.println("  Sending GET request to: " + baseUrl + endpoint);
             
             // In a real implementation, this would use HttpClient or similar
             
             // Simulate successful response
             Map<String, Object> responseData = new HashMap<>();
             responseData.put("positions", 5);
             responseData.put("location", "Multiple locations");
             
             return new ApiResponse(200, responseData);
         }
         
         public ApiResponse post(String endpoint, Map<String, Object> data) throws ApiException {
             // Simulate HTTP POST request
             System.out.println("  Sending POST request to: " + baseUrl + endpoint);
             System.out.println("  Payload: " + data);
             
             // In a real implementation, this would use HttpClient or similar
             
             // Simulate successful response
             Map<String, Object> responseData = new HashMap<>();
             responseData.put("applicationId", "APP-" + UUID.randomUUID().toString().substring(0, 8));
             responseData.put("submissionTime", LocalDateTime.now().toString());
             responseData.put("status", "RECEIVED");
             
             return new ApiResponse(201, responseData);
         }
     }
     
     // API Response class
     static class ApiResponse {
         private final int statusCode;
         private final Map<String, Object> data;
         
         public ApiResponse(int statusCode, Map<String, Object> data) {
             this.statusCode = statusCode;
             this.data = data;
         }
         
         public int getStatusCode() {
             return statusCode;
         }
         
         public Map<String, Object> getData() {
             return data;
         }
     }
     
     // Custom API Exception
     static class ApiException extends Exception {
         public ApiException(String message) {
             super(message);
         }
     }
     
     // Custom Application Exception for demonstration
     static class CustomApplicationException extends Exception {
         public CustomApplicationException(String message) {
             super(message);
         }
     }
     
     // Demonstrate proper exception handling
     private static void demonstrateExceptionHandling() throws CustomApplicationException {
         try {
             // Simulate potential exceptions
             System.out.println("Attempting to process application data...");
             
             Random random = new Random();
             int value = random.nextInt(3);
             
             if (value == 0) {
                 throw new IOException("Could not read configuration file");
             } else if (value == 1) {
                 throw new IllegalArgumentException("Invalid parameter in application data");
             } else {
                 System.out.println("Application data processed successfully");
             }
         } catch (IOException e) {
             System.out.println("Handled I/O exception: " + e.getMessage());
             System.out.println("Attempting recovery...");
             // Recovery logic would go here
         } catch (IllegalArgumentException e) {
             System.out.println("Handled validation exception: " + e.getMessage());
             throw new CustomApplicationException("Application processing failed: " + e.getMessage());
         }
     }
     
     // Simple logger class (in a real app, would use Log4j or similar)
     static class Logger {
         private final String className;
         
         private Logger(String className) {
             this.className = className;
         }
         
         public static Logger getLogger(String className) {
             return new Logger(className);
         }
         
         public void log(Level level, String message, Throwable throwable) {
             System.out.println("[" + level + "] " + className + ": " + message);
             if (throwable != null) {
                 throwable.printStackTrace();
             }
         }
     }
     
     // Logging level enum
     enum Level {
         INFO, WARNING, SEVERE
     }
 }
