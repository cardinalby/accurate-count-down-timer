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
9999
8997
7995
6993
5991
4987
3985
2979
1975
971
```
As you can see it increases it's lag by 2-3 milliseconds each tick. The reason is that 
[it's implementation](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/os/CountDownTimer.java)
doesn't take into account the time a message stays in thread's message queue and the time needed 
for [synchronization](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/os/CountDownTimer.java#L119).
