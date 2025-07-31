# Lwz-
Compress
import java.util.*;

class LZWCompress {

    public static List<Integer> compress(String input) {

        Map<String, Integer> dict = new HashMap<>();

        // Initialize dictionary with ASCII characters
        for (int i = 0; i < 256; i++) {
            dict.put("" + (char) i, i);
        }

        String w = "";
        List<Integer> result = new ArrayList<>();
        int code = 256;

        for (char c : input.toCharArray()) {
            String wc = w + c;

            if (dict.containsKey(wc)) {
                w = wc;
            } else {
                result.add(dict.get(w));
                dict.put(wc, code++);
                w = "" + c;
            }
        }

        // Add the last w to result
        if (!w.equals("")) {
            result.add(dict.get(w));
        }

        return result;
    }

    public static void main(String[] args) {

        String input = "AAABABABABB";

        List<Integer> result = compress(input);

        // Print the result
        System.out.println("Compressed Output:");
        for (int code : result) {
            System.out.print(code + " ");
        }
        System.out.println(); // Add newline
    }
}

// decompress
import java.util.*;

public class LZWDecompress {

    public static String decompress(List<Integer> input) {
        Map<Integer, String> dict = new HashMap<>();

        // Initialize dictionary with ASCII characters
        for (int i = 0; i < 256; i++) {
            dict.put(i, "" + (char) i);
        }

        String w = dict.get(input.get(0));
        StringBuilder result = new StringBuilder(w);
        int code = 256;

        for (int i = 1; i < input.size(); i++) {
            int k = input.get(i);
            String entry;

            if (dict.containsKey(k)) {
                entry = dict.get(k);
            } else {
                // Special case when k is not in the dictionary
                entry = w + w.charAt(0);
            }

            result.append(entry);
            dict.put(code++, w + entry.charAt(0));
            w = entry;
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Sample compressed input (must match valid LZW codes)
        List<Integer> codes = Arrays.asList(65, 256, 66, 258, 260, 66);

        String output = decompress(codes);
        System.out.println("LZW Decoded: " + output);
    }
}
