---
id: KG26w5TYcjaCg0Nbyspxg
title: Create Java Class
desc: ''
updated: 1640017698418
created: 1640017685126
---

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
