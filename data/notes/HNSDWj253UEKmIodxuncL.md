
Compile with JNI headers using `g++`
```
g++ -c -fPIC -I${JAVA_HOME}/include -I${JAVA_HOME}/include/linux ApplyWeights.cpp -o ApplyWeights.o
```
It will generate `ApplyWeights.o`. Then link it into bridge library

```
g++ -shared -fPIC -o libapplyWeights.so ApplyWeights.o -lc
```
It will generate `libapplyWeights.so`. The last is run the program with full path of library directory
```
java -cp . -Djava.library.path=/home/ruichenzheng/jni_test ApplyWeights
```
Command line output:
```
Initializing...
Weight 0: 10
Weight 4: 6
```