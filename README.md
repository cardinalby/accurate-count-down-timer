[![](https://jitpack.io/v/cardinalby/accurate-count-down-timer.svg)](https://jitpack.io/#cardinalby/accurate-count-down-timer)

# Accurate version of Android CountDownTimer

## Usage

The same as [Android CountDownTimer](https://developer.android.com/reference/android/os/CountDownTimer).

## Download

Add it in your root build.gradle at the end of repositories:
```groovy
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Add the dependency:

```groovy
dependencies {
    implementation 'com.github.cardinalby:accurate-count-down-timer:1.0'
}
```

# Details

If you start default `CountDownTimer` (`new CountDownTimer(10000, 1000)`) and log 
`millisUntilFinished` in `onTick` you will observe something like this:
```
9999        // 1 ms lag
8997        // 3 ms lag
7995        // 5 ms lag
6993        // 7 ms lag
5991        // 9 ms lag
4987        // 13 ms lag
3985        // 15 ms lag
2979        // 21 ms lag
1975        // 25 ms lag
971         // 29 ms lag
```
As you can see it increases it's lag by 2-3 milliseconds each tick. The reason is that 
[it's implementation](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/os/CountDownTimer.java)
doesn't take into account the time a message stays in thread's message queue and the time needed 
for [synchronization](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/os/CountDownTimer.java#L119).

Check out [my fix](https://github.com/cardinalby/accurate-count-down-timer/commit/8111b39815c36741e869a1297a66ad4efac7e228?branch=8111b39815c36741e869a1297a66ad4efac7e228&diff=unified)
and results of the same test after applying it:
```
9999        // 1 ms lag
8999        // 1 ms lag
7999        // 1 ms lag
6997        // 3 ms lag
5997        // 3 ms lag
4998        // 2 ms lag
3997        // 3 ms lag
2998        // 2 ms lag
1997        // 3 ms lag
997         // 3 ms lag
```
Now you can see that although delay is still here it doesn't increase. The delay is caused by the 
time message stays in message queue until it gets processed by Looper and can't be precisely 
predicted to be taken into account.

