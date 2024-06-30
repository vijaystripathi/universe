## 📌 BloomFilter

package algorithms;
import java.util.BitSet;
import java.util.Random;

public class BloomFilterImpl {
    private final BitSet bitSet;
    private final int bitSetSize;
    private final int[] hashSeeds;
    private final int numberOfHashFunctions;
    private final Random random;

    public BloomFilterImpl(int bitSetSize, int numberOfHashFunctions) {
        this.bitSetSize = bitSetSize;
        this.numberOfHashFunctions = numberOfHashFunctions;
        this.bitSet = new BitSet(bitSetSize);
        this.hashSeeds = new int[numberOfHashFunctions];
        this.random = new Random();

        // Generate random seeds for hash functions
        for (int i = 0; i < numberOfHashFunctions; i++) {
            hashSeeds[i] = random.nextInt();
        }
    }

    public void addingElementToSet(String value) {
        for (int seed : hashSeeds) {
            int hash = hash(value, seed);
            bitSet.set(Math.abs(hash % bitSetSize), true);
        }
    }

    public boolean probablyPresentInSet(String value) {
        for (int seed : hashSeeds) {
            int hash = hash(value, seed);
            if (!bitSet.get(Math.abs(hash % bitSetSize))) {
                return false;
            }
        }
        return true;
    }

    private int hash(String value, int seed) {
        int result = 0;
        for (char c : value.toCharArray()) {
            result = seed * result + c;
        }
        return result;
    }

    public static void main(String[] args) {
        int bitSetSize = 1024;
        int numberOfHashFunctions = 3;

        BloomFilterImpl bloomFilter = new BloomFilterImpl(bitSetSize, numberOfHashFunctions);

        bloomFilter.addingElementToSet("red");
        bloomFilter.addingElementToSet("blue");
        bloomFilter.addingElementToSet("black");

        System.out.println(bloomFilter.probablyPresentInSet("red")); // true
        System.out.println(bloomFilter.probablyPresentInSet("blue")); // true
        System.out.println(bloomFilter.probablyPresentInSet("black")); // true
        System.out.println(bloomFilter.probablyPresentInSet("green")); // false
    }
}
