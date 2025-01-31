# Flutter Project Dependencies Documentation

This document provides an overview of all packages used in this project, their purposes, and basic usage examples.

## Core Dependencies

### Firebase Suite

- **firebase_core** (^2.24.2)

  - Base Firebase package required for all Firebase services
  - Initializes Firebase in the application

  ```dart
  void main() async {
    WidgetsFlutterBinding.ensureInitialized();
    await Firebase.initializeApp(
      options: DefaultFirebaseOptions.currentPlatform,
    );
    runApp(MyApp());
  }
  ```

- **firebase_auth** (^4.16.0)

  - Handles user authentication
  - Supports email/password, phone, and social auth

  ```dart
  // Sign in with email
  final credential = await FirebaseAuth.instance.signInWithEmailAndPassword(
    email: email,
    password: password,
  );

  // Get current user
  final user = FirebaseAuth.instance.currentUser;
  ```

- **cloud_firestore** (^4.14.0)

  - NoSQL database for Firebase
  - Store and sync data in real-time

  ```dart
  // Add data
  await FirebaseFirestore.instance.collection('users').add({
    'name': 'John Doe',
    'age': 30,
  });

  // Read data
  final snapshot = await FirebaseFirestore.instance
      .collection('users')
      .where('age', isGreaterThan: 20)
      .get();
  ```

### State Management

- **flutter_hooks** (^0.20.5)

  - Provides React-like hooks for Flutter
  - Manages widget lifecycle and state

  ```dart
  class ExampleWidget extends HookWidget {
    @override
    Widget build(BuildContext context) {
      final counter = useState(0);

      return TextButton(
        onPressed: () => counter.value++,
        child: Text('Count: ${counter.value}'),
      );
    }
  }
  ```

- **hooks_riverpod** (^2.6.1)

  - Combines Riverpod state management with Flutter Hooks
  - Provides dependency injection and state management

  ```dart
  // Define a provider
  final counterProvider = StateNotifierProvider<Counter, int>((ref) => Counter());

  // Use in widget
  class ExampleWidget extends HookConsumerWidget {
    @override
    Widget build(BuildContext context, WidgetRef ref) {
      final count = ref.watch(counterProvider);
      return Text('Count: $count');
    }
  }
  ```

### Navigation

- **go_router** (^14.7.2)
  - Declarative routing package
  - Supports deep linking and URL patterns
  ```dart
  final router = GoRouter(
    routes: [
      GoRoute(
        path: '/',
        builder: (context, state) => HomeScreen(),
      ),
      GoRoute(
        path: '/user/:id',
        builder: (context, state) => UserProfile(
          id: state.pathParameters['id']!,
        ),
      ),
    ],
  );
  ```

### Network & Data

- **http** (^1.3.0)

  - Basic HTTP requests
  - RESTful API calls

  ```dart
  final response = await http.get(Uri.parse('https://api.example.com/data'));
  if (response.statusCode == 200) {
    final data = jsonDecode(response.body);
  }
  ```

- **shared_preferences** (^2.5.1)
  - Persistent key-value storage
  - Local data caching
  ```dart
  final prefs = await SharedPreferences.getInstance();
  await prefs.setString('user_token', 'abc123');
  final token = prefs.getString('user_token');
  ```

### UI Components

- **flutter_svg** (^2.0.17)

  - SVG rendering support

  ```dart
  SvgPicture.asset(
    'assets/icon.svg',
    width: 100,
    height: 100,
  )
  ```

- **lottie** (^3.3.1)

  - Renders After Effects animations

  ```dart
  Lottie.asset(
    'assets/animation.json',
    width: 200,
    height: 200,
    fit: BoxFit.fill,
  )
  ```

- **google_fonts** (^6.2.1)
  - Easy access to Google Fonts
  ```dart
  Text(
    'Hello World',
    style: GoogleFonts.roboto(
      fontSize: 24,
      fontWeight: FontWeight.bold,
    ),
  )
  ```

### Device Features

- **sensors_plus** (^6.1.1)

  - Access device sensors

  ```dart
  accelerometerEvents.listen((AccelerometerEvent event) {
    print('X: ${event.x}, Y: ${event.y}, Z: ${event.z}');
  });
  ```

- **connectivity_plus** (^6.1.2)
  - Monitor network connectivity
  ```dart
  final connectivity = await Connectivity().checkConnectivity();
  if (connectivity == ConnectivityResult.wifi) {
    print('Connected to WiFi');
  }
  ```

### Data Handling

- **json_annotation** & **json_serializable**

  - JSON serialization/deserialization

  ```dart
  @JsonSerializable()
  class User {
    final String name;
    final int age;

    User(this.name, this.age);

    factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
    Map<String, dynamic> toJson() => _$UserToJson(this);
  }
  ```

- **freezed_annotation** & **freezed**

  - Immutable class generation

  ```dart
  @freezed
  class Person with _$Person {
    factory Person({
      required String name,
      required int age,
    }) = _Person;

    factory Person.fromJson(Map<String, dynamic> json) => _$PersonFromJson(json);
  }
  ```

### File System & Permissions

- **path_provider** (^2.1.5)

  - Access system directories

  ```dart
  final directory = await getApplicationDocumentsDirectory();
  final path = directory.path;
  ```

- **permission_handler** (^11.3.1)
  - Handle runtime permissions
  ```dart
  if (await Permission.camera.request().isGranted) {
    // Camera permission granted
  }
  ```

### Environment Variables

- **flutter_dotenv** (^5.2.1)
  - Load environment variables
  ```dart
  await dotenv.load();
  final apiKey = dotenv.get('API_KEY');
  ```

## Dev Dependencies

- **build_runner**

  - Code generation tool
  - Run with: `flutter pub run build_runner build`

- **mockito**

  - Mocking framework for tests

  ```dart
  class MockHttpClient extends Mock implements http.Client {}
  ```

- **flutter_launcher_icons**

  - Customize app icons
  - Configure in pubspec.yaml

- **flutter_native_splash**
  - Customize splash screen
  - Configure in pubspec.yaml

## Getting Started

1. Install dependencies:
   bash
   flutter pub get

2. Run code generation (if needed):
   bash
   flutter pub run build_runner build --delete-conflicting-outputs

3. Set up environment variables:
   - Copy `.env.example` to `.env`
   - Fill in required values

## Best Practices

1. Always check package documentation for breaking changes when updating
2. Use const constructors when possible
3. Implement proper error handling for network calls
4. Follow provider patterns for state management
5. Use proper dependency injection
