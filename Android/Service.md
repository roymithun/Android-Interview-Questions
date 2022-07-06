# Interview Questions on Service

1. [About service as a component](https://developer.android.com/guide/components/services)
    * A Service is an application component that can perform long-running operations in the background. It does not
      provide a user interface. Once started, a service might continue running for some time, even after the user
      switches to another application. Additionally, a component can bind to a service to interact with it and even
      perform interprocess communication (IPC).
    * A service runs in the main thread of its hosting process; the service does not create its own thread and does not
      run in a separate process unless you specify otherwise. You should run any blocking operations on a separate
      thread within the service to avoid Application Not Responding (ANR) errors.

2. [Types of services](https://developer.android.com/guide/components/services#Types-of-services)
    * [Foreground](https://developer.android.com/guide/components/foreground-services)
        * A foreground service performs some operation that is noticeable to the user. For example, an audio app would
          use a foreground service to play an audio track.
        * They continue running even when the user isn't interacting with the app.
        * When you use a foreground service, you must display a notification so that users are actively aware that the
          service is running. This notification cannot be dismissed unless the service is either stopped or removed from
          the foreground.
        * Apps that target Android 9 (API level 28) or higher and use foreground services must
          request [Foreground service permission](https://developer.android.com/guide/components/foreground-services#request-foreground-service-permissions)
           ```
             <manifest xmlns:android="http://schemas.android.com/apk/res/android" ...>
               <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
               <application ...>
                 ...
               </application>
            </manifest>
          ```
        * Restrictions on background starts
            * Apps that target Android 12 (API level 31) or higher can't start foreground services while running in the
              background, except
              for [a few special cases](https://developer.android.com/guide/components/foreground-services#background-start-restriction-exemptions)
              . If an app tries to start a foreground service while the app is running in the background, and the
              foreground service doesn't satisfy one of the exceptional cases, the system throws
              a [ForegroundServiceStartNotAllowedException](https://developer.android.com/reference/android/app/ForegroundServiceStartNotAllowedException)
              .
    * [Background](https://developer.android.com/training/run-background-service/create-service)
        * A background service performs an operation that isn't directly noticed by the user. For example, if an app
          used a service to compact its storage, that would usually be a background service and needs no UI notification
          awarness.
        * [If your app targets API level 26 or higher, the system imposes restrictions on running background services when the app itself isn't in the foreground.](https://developer.android.com/about/versions/oreo/background#services)
    * [Bound](https://developer.android.com/guide/components/bound-services)
        - A service is bound when an application component binds to it by calling bindService().
        - A bound service offers a client-server interface that allows components to interact with the service, send
          requests, receive results, and even do so across processes with interprocess communication (IPC).
        - A bound service runs only as long as another application component is bound to it. Multiple components can
          bind to the service at once, but when all of them unbind, the service is destroyed.