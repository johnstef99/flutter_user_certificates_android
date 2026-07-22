# flutter_user_certificates_android

[![pub package](https://img.shields.io/pub/v/flutter_user_certificates_android.svg)](https://pub.dev/packages/flutter_user_certificates_android)
[![pub points](https://img.shields.io/pub/points/flutter_user_certificates_android)](https://pub.dev/packages/flutter_user_certificates_android/score)
[![likes](https://img.shields.io/pub/likes/flutter_user_certificates_android)](https://pub.dev/packages/flutter_user_certificates_android/score)

A Flutter plugin for getting user installed certificates on Android.

## Getting Started

Here is an example that lists all the user installed certificates on the device.

```dart
import 'package:flutter/material.dart';
import 'dart:async';

import 'package:flutter/services.dart';
import 'package:flutter_user_certificates_android/flutter_user_certificates_android.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  Map<String, Uint8List> _certs = {};
  String? error;
  final _flutterUserCertificatesAndroidPlugin =
      FlutterUserCertificatesAndroid();

  @override
  void initState() {
    super.initState();
    initPlatformState();
  }

  Future<void> initPlatformState() async {
    Map<String, Uint8List>? certs;
    try {
      certs = await _flutterUserCertificatesAndroidPlugin.getUserCertificates();
    } on PlatformException {
      error = 'Failed to get user certificates from device.';
    }

    if (!mounted) return;

    if (certs == null) return;

    setState(() {
      _certs = certs!;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Plugin example app'),
        ),
        body: error != null
            ? Center(
                child: Text(error!),
              )
            : ListView.builder(
                itemCount: _certs.length,
                itemBuilder: (c, i) => ListTile(
                  title: Text(_certs.keys.elementAt(i)),
                ),
              ),
      ),
    );
  }
}
```
