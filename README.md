# Date-package User;
import java.util.*;

class User {
    String name;
    String gender;
    int age;
    Set<String> interests;

    public User(String name, String gender, int age, String... interests) {
        this.name = name;
        this.gender = gender;
        this.age = age;
        this.interests = new HashSet<>(Arrays.asList(interests));
    }

    @Override
    public String toString() {
        return name;
    }
}

class DatingRecommendationEngine {
    public static List<User> findBestMatches(User user, List<User> allUsers, int topN) {
        return allUsers.stream()
                .filter(u -> !u.name.equals(user.name))
                .sorted(Comparator.comparingInt((User u) -> getMatchScore(user, u)).reversed())
                .limit(topN)
                .toList();
    }

    private static int getMatchScore(User user, User potentialMatch) {
        int score = 0;
        // Gender Rule
        if (!user.gender.equals(potentialMatch.gender)) score += 100;
        // Age Rule
        score += 50 - Math.abs(user.age - potentialMatch.age);
        // Interest Rule
        score += Collections.frequency(user.interests, potentialMatch.interests);
        return score;
    }

    public static void main(String[] args) {
        List<User> users = Arrays.asList(
            new User("User 1", "Female", 25, "Cricket", "Chess"),
            new User("User 2", "Male", 27, "Cricket", "Football", "Movies"),
            new User("User 3", "Male", 26, "Movies", "Tennis", "Football", "Cricket"),
            new User("User 4", "Female", 24, "Tennis", "Football", "Badminton"),
            new User("User 5", "Female", 32, "Cricket", "Football", "Movies", "Badminton")
        );

        User currentUser = users.get(1); // User 2
        List<User> matches = findBestMatches(currentUser, users, 2);
        System.out.println("Top matches for " + currentUser.name + ": " + matches);
    }
}
