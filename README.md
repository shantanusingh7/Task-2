# Task-2
This implementation includes a basic hash function using SHA-256 for generating short URLs, a simple command-line interface (CLI) for user interaction, and error handling for invalid URLs and duplicate entries.
import java.util.HashMap;
import java.util.Map;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;
import java.util.Scanner;

public class LinkShortener {

    private Map<String, String> shortToLongMap;
    private Map<String, String> longToShortMap;

    public LinkShortener() {
        shortToLongMap = new HashMap<>();
        longToShortMap = new HashMap<>();
    }

    // Method to generate a short link for a given long URL
    public String shorten(String longUrl) {
        if (!isValidUrl(longUrl)) {
            return "Invalid URL. Please enter a valid URL.";
        }

        if (longToShortMap.containsKey(longUrl)) {
            return "Short URL already exists: " + longToShortMap.get(longUrl);
        }

        String shortUrl = generateShortUrl(longUrl);
        shortToLongMap.put(shortUrl, longUrl);
        longToShortMap.put(longUrl, shortUrl);

        return "Shortened URL: " + shortUrl;
    }

    // Method to expand a short link to its original long URL
    public String expand(String shortUrl) {
        if (!isValidShortUrl(shortUrl)) {
            return "Invalid short URL. Please enter a valid short URL.";
        }

        if (!shortToLongMap.containsKey(shortUrl)) {
            return "Short URL not found";
        }

        return "Expanded URL: " + shortToLongMap.get(shortUrl);
    }

    // Method to check if a given string is a valid URL
    private boolean isValidUrl(String url) {
        // Implement your URL validation logic here
        // For simplicity, we assume any non-empty string is a valid URL
        return url != null && !url.isEmpty();
    }

    // Method to check if a given string is a valid short URL
    private boolean isValidShortUrl(String shortUrl) {
        // Implement your short URL validation logic here
        // For simplicity, we assume any non-empty string is a valid short URL
        return shortUrl != null && !shortUrl.isEmpty();
    }

    // Method to generate a short URL using a basic hash function
    private String generateShortUrl(String longUrl) {
        try {
            MessageDigest md = MessageDigest.getInstance("SHA-256");
            byte[] hash = md.digest(longUrl.getBytes());
            String encoded = Base64.getUrlEncoder().encodeToString(hash);
            return encoded.substring(0, 8); // Use the first 8 characters as a short URL
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
            return "";
        }
    }

    public static void main(String[] args) {
        LinkShortener linkShortener = new LinkShortener();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("1. Shorten URL");
            System.out.println("2. Expand URL");
            System.out.println("3. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter the long URL to shorten: ");
                    String longUrl = scanner.nextLine();
                    String result = linkShortener.shorten(longUrl);
                    System.out.println(result);
                    break;
                case 2:
                    System.out.print("Enter the short URL to expand: ");
                    String shortUrlToExpand = scanner.nextLine();
                    String expandedUrlResult = linkShortener.expand(shortUrlToExpand);
                    System.out.println(expandedUrlResult);
                    break;
                case 3:
                    System.out.println("Exiting...");
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice. Please choose again.");
            }
        }
    }
}
