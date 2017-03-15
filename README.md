# stream_reader

[![version 1.0.1](https://img.shields.io/badge/pub-1.0.1-brightgreen.svg)](https://pub.dartlang.org/packages/stream_reader)
[![build status](https://travis-ci.org/thosakwe/stream_reader.svg)](https://travis-ci.org/thosakwe/stream_reader)

Asynchronously read from Dart streams. Supports peeking, reading, and a reference
to the current element.

# Usage
```dart
import 'dart:async';
import 'package:stream_reader/stream_reader.dart';
import 'package:test/test.dart';

Stream<String> strings() async* {
  yield 'Michael';
  yield 'Jackson';
  yield 'Bernie';
  yield 'Sanders';
}

main() {
  test('read', () async {
    // Read two
    var reader = new StreamReader<String>()..addStream(strings());
    await reader.consume();
    var str = await reader.consume();
    expect(str, equals('Jackson'));
  });

  test('peek+current', () async {
    // Read two
    var reader = new StreamReader<String>()..addStream(strings());
    await reader.consume();
    await reader.consume();
    var peek = await reader.peek();
    expect(peek, equals('Bernie'));

    // current should still be second string
    var str = await reader.current();
    expect(str, equals('Jackson'));
  });
}
```