
> ApplyWeights.java

```
public class ApplyWeights {

    static {
        System.loadLibrary("applyWeights");
    }
    
    public static void main(String[] args) {

        float weights[] = new float[8];

        weights[0] = 10f;
        weights[4] = 6f;
        
        new ApplyWeights().applyWeights(weights);
    }

    // Declare a native method sayHello() that receives no arguments and returns void
    private native void applyWeights(float[] weights);
}
```
