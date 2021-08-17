---
title: Synopsis Flutter
---

## Dart


<br />

## State management

<br />

### Stateless vs stateful 

<br />

## Software design patterns

[Stack overflow: how to vuild a singleton in dart](https://stackoverflow.com/questions/12649573/how-do-you-build-a-singleton-in-dart)

```dart
class Singleton {
  static final Singleton _singleton = Singleton._internal();

  factory Singleton() {
    return _singleton;
  }

  Singleton._internal();
}
```

<br/ >
