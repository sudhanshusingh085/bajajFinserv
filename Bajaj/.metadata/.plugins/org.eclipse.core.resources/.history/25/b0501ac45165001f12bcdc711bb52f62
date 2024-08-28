package org.example;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.google.gson.JsonPrimitive;

import java.io.FileReader;
import java.io.IOException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Map;
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        if (args.length < 2) {
            System.out.println("Usage: java -jar bajaj.jar <PRN Number> <JSON file path>");
            return;
        }

        String prnNumber = args[0].toLowerCase().replaceAll("\\s", "");
        String jsonFilePath = args[1];

        try {
            String destinationValue = findDestinationValue(jsonFilePath);
            if (destinationValue == null) {
                System.out.println("Key 'destination' not found in the JSON file.");
                return;
            }

            String randomString = generateRandomString(8);
            String concatenatedString = prnNumber + destinationValue + randomString;
            String hashValue = generateMD5Hash(concatenatedString);

            System.out.println(hashValue + ";" + randomString);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String findDestinationValue(String jsonFilePath) throws IOException {
        JsonElement rootElement = JsonParser.parseReader(new FileReader(jsonFilePath));
        return searchForDestinationKey(rootElement);
    }

    private static String searchForDestinationKey(JsonElement element) {
        if (element.isJsonObject()) {
            JsonObject jsonObject = element.getAsJsonObject();
            for (Map.Entry<String, JsonElement> entry : jsonObject.entrySet()) {
                if (entry.getKey().equals("destination")) {
                    return entry.getValue().getAsString();
                }
                String result = searchForDestinationKey(entry.getValue());
                if (result != null) {
                    return result;
                }
            }
        } else if (element.isJsonArray()) {
            for (JsonElement arrayElement : element.getAsJsonArray()) {
                String result = searchForDestinationKey(arrayElement);
                if (result != null) {
                    return result;
                }
            }
        }
        return null;
    }

    private static String generateRandomString(int length) {
        String chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        Random random = new Random();
        StringBuilder sb = new StringBuilder(length);
        for (int i = 0; i < length; i++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }
        return sb.toString();
    }

    private static String generateMD5Hash(String input) throws NoSuchAlgorithmException {
        MessageDigest md = MessageDigest.getInstance("MD5");
        byte[] messageDigest = md.digest(input.getBytes());
        StringBuilder sb = new StringBuilder();
        for (byte b : messageDigest) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}