name: "Flutter Vocabulary Learner - Complete PRP v2"
description: |

## Purpose
Comprehensive implementation guide for a Flutter-based vocabulary learning application targeting mastery of 7000 most commonly used English vocabulary words through daily testing and spaced repetition principles.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from Flutter best practices
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build a Flutter mobile application that helps students master 7000 most commonly used English vocabulary words through a simple, effective daily testing interface with "yes/no" familiarity assessment, progress tracking, and adaptive learning algorithms.

## Why
- **Business value**: Addresses the critical need for vocabulary mastery in English language learning
- **User impact**: Provides a simple, non-intimidating interface for vocabulary assessment and learning
- **Integration**: Serves as a practical implementation example of Flutter best practices and architecture patterns
- **Problem solved**: Eliminates the complexity of traditional vocabulary learning tools while providing effective spaced repetition learning

## What
A mobile application with the following user-visible behavior and technical requirements:

### User Experience
- Clean, minimal interface showing one word at a time
- Simple "YES" (familiar) / "NO" (unfamiliar) buttons for word assessment
- Daily generation of 50 words for testing from the 7000-word corpus
- Progress tracking with visual indicators
- Dark/light theme toggle
- Adaptive algorithm that prioritizes unknown words

### Technical Requirements
- Flutter framework with cross-platform support (iOS/Android)
- Local storage for word database and user progress
- Efficient daily word selection algorithm
- State management for app-wide data flow
- Responsive UI with material design principles
- Offline-first architecture

### Success Criteria
- [ ] User can complete a daily 50-word vocabulary test in under 5 minutes
- [ ] App tracks user progress with persistent storage
- [ ] Words marked as "unknown" appear more frequently in future tests
- [ ] App supports both dark and light themes with smooth transitions
- [ ] App works completely offline after initial setup
- [ ] User can view progress statistics and learning analytics
- [ ] App follows Flutter best practices and passes all linting/testing

## All Needed Context

### Documentation & References (list all context needed to implement the feature)
```yaml
# MUST READ - Include these in your context window
- url: https://docs.flutter.dev/app-architecture
  why: Official Flutter architecture guidance and MVVM pattern implementation
  
- url: https://docs.flutter.dev/app-architecture/design-patterns
  why: Architecture patterns for scalable Flutter applications
  
- url: https://docs.flutter.dev/cookbook/persistence/sqlite
  why: SQLite implementation patterns for local data storage
  
- url: https://pub.dev/packages/hive
  why: Alternative lightweight NoSQL database for simple key-value storage
  
- url: https://pub.dev/packages/riverpod
  why: Modern state management solution with compile-time safety
  
- url: https://pub.dev/packages/flutter_bloc
  why: Alternative BLoC pattern state management for structured event/state flows
  
- url: https://github.com/first20hours/google-10000-english
  why: Primary data source for 7000 most common English words (frequency-sorted)
  
- url: https://docs.flutter.dev/ui/adaptive-responsive
  why: Responsive design patterns for different screen sizes
  
- url: https://docs.flutter.dev/ui/design/themes
  why: Theming implementation for dark/light mode support
  
- doc: https://gwern.net/spaced-repetition
  section: Spaced repetition algorithms and implementation theory
  critical: Understanding forgetting curves and optimal review intervals
```

### Current Codebase tree (run `tree` in the root of the project) to get an overview of the codebase
```bash
ai-vocab-learner/
├── .claude/
│   ├── commands/
│   │   ├── generate-prp.md
│   │   └── execute-prp.md
│   └── settings.local.json
├── PRPs/
│   ├── templates/
│   │   └── prp_base.md
│   └── EXAMPLE_multi_agent_prp.md
├── examples/ (empty - to be populated)
├── use-cases/
│   └── mcp-server/ (TypeScript/Node.js patterns)
├── CLAUDE.md (project rules)
├── INITIAL.md (feature requirements)
├── INITIAL_EXAMPLE.md
├── LICENSE
└── README.md
```

### Desired Codebase tree with files to be added and responsibility of file
```bash
ai-vocab-learner/
├── flutter_vocab_app/
│   ├── android/
│   ├── ios/
│   ├── lib/
│   │   ├── main.dart (App entry point, theme setup)
│   │   ├── models/
│   │   │   ├── word.dart (Word data model)
│   │   │   ├── user_progress.dart (Progress tracking model)
│   │   │   └── daily_session.dart (Daily test session model)
│   │   ├── services/
│   │   │   ├── database_service.dart (Local storage abstraction)
│   │   │   ├── word_service.dart (Word selection algorithms)
│   │   │   └── progress_service.dart (Progress tracking logic)
│   │   ├── providers/
│   │   │   ├── theme_provider.dart (Dark/light theme state)
│   │   │   ├── progress_provider.dart (User progress state)
│   │   │   └── word_test_provider.dart (Daily test state)
│   │   ├── screens/
│   │   │   ├── home_screen.dart (Main navigation hub)
│   │   │   ├── word_test_screen.dart (Daily vocabulary test UI)
│   │   │   ├── progress_screen.dart (Statistics and analytics)
│   │   │   └── settings_screen.dart (Theme toggle, preferences)
│   │   ├── widgets/
│   │   │   ├── word_card.dart (Individual word display component)
│   │   │   ├── progress_indicator.dart (Custom progress visualization)
│   │   │   └── theme_toggle.dart (Dark/light mode switch)
│   │   └── utils/
│   │       ├── constants.dart (App constants, colors, strings)
│   │       └── spaced_repetition.dart (Learning algorithm implementation)
│   ├── assets/
│   │   └── data/
│   │       └── common_words.json (7000 word dataset)
│   ├── test/
│   │   ├── models/
│   │   ├── services/
│   │   ├── providers/
│   │   └── widgets/
│   ├── pubspec.yaml (Dependencies and configuration)
│   ├── pubspec.lock
│   └── README.md
├── examples/
│   └── flutter_patterns/ (Code examples for reference)
└── (existing Context Engineering files...)
```

### Known Gotchas of our codebase & Library Quirks
```dart
// CRITICAL: Flutter requires async/await for database operations
// Example: All SQLite operations must be awaited
await database.insert('words', word.toMap());

// CRITICAL: Riverpod providers must be declared globally
final wordTestProvider = StateNotifierProvider<WordTestNotifier, WordTestState>(...);

// CRITICAL: Theme changes require context and must use Provider.of or Consumer
Theme.of(context).brightness == Brightness.dark

// CRITICAL: Asset loading requires proper pubspec.yaml configuration
assets:
  - assets/data/common_words.json

// CRITICAL: Hive requires type adapters for custom objects
@HiveType(typeId: 0)
class Word extends HiveObject {
  @HiveField(0)
  String word;
  @HiveField(1)  
  int difficulty;
}

// CRITICAL: Flutter testing requires specific test imports
import 'package:flutter_test/flutter_test.dart';
import 'package:flutter/material.dart';
```

## Implementation Blueprint

### Data models and structure

Create the core data models to ensure type safety and consistency.
```dart
// Core data models for vocabulary learning system
// Word model with frequency ranking and user interaction tracking
// UserProgress model with statistics and learning analytics  
// DailySession model for managing daily 50-word tests
// Spaced repetition algorithm integration for adaptive learning

// Pydantic-style validation using Dart's built-in features
class Word {
  final String word;
  final int frequencyRank; // 1-7000 based on Google corpus
  final DateTime lastSeen;
  final int correctCount;
  final int totalCount;
  
  // Validation in constructor
  Word({required this.word, required this.frequencyRank}) {
    if (word.isEmpty) throw ArgumentError('Word cannot be empty');
    if (frequencyRank < 1 || frequencyRank > 7000) {
      throw ArgumentError('Frequency rank must be 1-7000');
    }
  }
}
```

### list of tasks to be completed to fullfill the PRP in the order they should be completed

```yaml
Task 1: Project Setup and Data Preparation
CREATE flutter_vocab_app/:
  - Run: flutter create flutter_vocab_app
  - MODIFY pubspec.yaml: Add dependencies (riverpod, hive, path_provider)
  - CREATE assets/data/common_words.json: Download and format 7000 words from GitHub

Task 2: Core Data Models
CREATE lib/models/:
  - CREATE word.dart: Word data model with validation
  - CREATE user_progress.dart: Progress tracking with statistics
  - CREATE daily_session.dart: Daily test session management

Task 3: Database and Storage Services  
CREATE lib/services/:
  - CREATE database_service.dart: Hive setup and operations
  - CREATE word_service.dart: Word selection and filtering algorithms
  - CREATE progress_service.dart: Progress tracking and analytics

Task 4: State Management Setup
CREATE lib/providers/:
  - CREATE theme_provider.dart: Dark/light theme state management
  - CREATE progress_provider.dart: User progress state with Riverpod
  - CREATE word_test_provider.dart: Daily test session state

Task 5: Core UI Screens
CREATE lib/screens/:
  - CREATE home_screen.dart: Main navigation and daily progress
  - CREATE word_test_screen.dart: Core vocabulary testing interface
  - CREATE progress_screen.dart: Statistics and learning analytics
  - CREATE settings_screen.dart: Theme toggle and preferences

Task 6: Custom Widgets
CREATE lib/widgets/:
  - CREATE word_card.dart: Individual word display with yes/no buttons
  - CREATE progress_indicator.dart: Custom progress visualization
  - CREATE theme_toggle.dart: Dark/light mode toggle switch

Task 7: Utility Functions and Algorithms
CREATE lib/utils/:
  - CREATE constants.dart: App constants, colors, theme definitions
  - CREATE spaced_repetition.dart: Learning algorithm implementation

Task 8: Main App Integration
MODIFY lib/main.dart:
  - INJECT Riverpod providers setup
  - CONFIGURE theme management
  - SET navigation routes

Task 9: Testing Implementation
CREATE test/:
  - CREATE unit tests for all models, services, and providers
  - CREATE widget tests for custom components
  - CREATE integration tests for complete user flows

Task 10: Final Polish and Optimization
OPTIMIZE performance:
  - IMPLEMENT lazy loading for word lists
  - OPTIMIZE database queries for daily word selection
  - ENSURE smooth animations and transitions
  - VALIDATE offline functionality
```

### Per task pseudocode as needed added to each task

```dart
// Task 1: Project Setup
flutter create flutter_vocab_app --platforms=android,ios
cd flutter_vocab_app

// CRITICAL: pubspec.yaml dependencies
dependencies:
  flutter:
    sdk: flutter
  riverpod: ^2.4.0
  flutter_riverpod: ^2.4.0
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  path_provider: ^2.1.1

// Task 2: Core Models Pattern
class Word {
  // PATTERN: Immutable data classes with validation
  final String word;
  final int frequencyRank;
  final DateTime? lastTested;
  final double familiarity; // 0.0 = unknown, 1.0 = known
  
  // PATTERN: Factory constructors for different creation scenarios
  factory Word.fromJson(Map<String, dynamic> json) => Word(...);
  Map<String, dynamic> toJson() => {...};
}

// Task 3: Database Service Pattern  
class DatabaseService {
  // PATTERN: Singleton pattern for database access
  static late Box<Word> _wordBox;
  static late Box<UserProgress> _progressBox;
  
  // CRITICAL: Initialize Hive before app starts
  static Future<void> init() async {
    await Hive.initFlutter();
    // Register adapters before opening boxes
    Hive.registerAdapter(WordAdapter());
    _wordBox = await Hive.openBox<Word>('words');
  }
}

// Task 4: State Management Pattern
final wordTestProvider = StateNotifierProvider<WordTestNotifier, WordTestState>((ref) {
  return WordTestNotifier(ref.read(databaseServiceProvider));
});

class WordTestNotifier extends StateNotifier<WordTestState> {
  // PATTERN: Business logic in StateNotifier
  // CRITICAL: Always emit new state objects for UI updates
  void markWordFamiliar(String word) {
    final updatedWords = state.dailyWords.map((w) => 
      w.word == word ? w.copyWith(familiarity: 1.0) : w
    ).toList();
    
    state = state.copyWith(dailyWords: updatedWords);
  }
}

// Task 5: Screen Architecture Pattern
class WordTestScreen extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final testState = ref.watch(wordTestProvider);
    
    // PATTERN: Consumer pattern for reactive UI
    return Scaffold(
      body: PageView.builder(
        itemCount: testState.dailyWords.length,
        itemBuilder: (context, index) {
          return WordCard(word: testState.dailyWords[index]);
        },
      ),
    );
  }
}

// Task 7: Spaced Repetition Algorithm
class SpacedRepetition {
  // PATTERN: Modified Leitner system for vocabulary
  static List<Word> selectDailyWords(List<Word> allWords, int count) {
    // CRITICAL: Prioritize unknown words (familiarity < 0.5)
    final unknownWords = allWords.where((w) => w.familiarity < 0.5).toList();
    final knownWords = allWords.where((w) => w.familiarity >= 0.5).toList();
    
    // ALGORITHM: 70% unknown, 30% review of known words
    final unknownCount = (count * 0.7).round();
    final reviewCount = count - unknownCount;
    
    return [
      ...unknownWords.take(unknownCount),
      ...knownWords.take(reviewCount)
    ];
  }
}
```

### Integration Points
```yaml
ASSETS:
  - add to: pubspec.yaml
  - pattern: |
    flutter:
      assets:
        - assets/data/common_words.json

NAVIGATION:
  - add to: lib/main.dart
  - pattern: |
    routes: {
      '/': (context) => HomeScreen(),
      '/test': (context) => WordTestScreen(),
      '/progress': (context) => ProgressScreen(),
      '/settings': (context) => SettingsScreen(),
    }

THEME:
  - add to: lib/main.dart  
  - pattern: |
    theme: ThemeData.light(),
    darkTheme: ThemeData.dark(),
    themeMode: ref.watch(themeProvider),

DATABASE:
  - initialization: main.dart before runApp()
  - pattern: |
    await DatabaseService.init();
    await WordService.loadWordsFromAssets();
```

## Validation Loop

### Level 1: Syntax & Style
```bash
# Run these FIRST - fix any errors before proceeding
cd flutter_vocab_app
flutter analyze                    # Static analysis
dart format lib/ --set-exit-if-changed  # Code formatting

# Expected: No errors. If errors, READ the error and fix.
```

### Level 2: Unit Tests each new feature/file/function use existing test patterns
```dart
// CREATE test/models/word_test.dart
void main() {
  group('Word Model Tests', () {
    test('should create word with valid parameters', () {
      final word = Word(word: 'hello', frequencyRank: 100);
      expect(word.word, equals('hello'));
      expect(word.frequencyRank, equals(100));
    });

    test('should throw error for empty word', () {
      expect(
        () => Word(word: '', frequencyRank: 100),
        throwsA(isA<ArgumentError>()),
      );
    });

    test('should throw error for invalid frequency rank', () {
      expect(
        () => Word(word: 'hello', frequencyRank: 8000),
        throwsA(isA<ArgumentError>()),
      );
    });
  });
}

// CREATE test/services/word_service_test.dart
void main() {
  group('WordService Tests', () {
    test('should select 50 daily words', () async {
      final words = await WordService.getDailyWords(50);
      expect(words.length, equals(50));
    });

    test('should prioritize unknown words', () async {
      // Mock data with mix of known/unknown words
      final words = await WordService.getDailyWords(10);
      final unknownCount = words.where((w) => w.familiarity < 0.5).length;
      expect(unknownCount, greaterThan(5)); // At least 50% unknown
    });
  });
}
```

```bash
# Run and iterate until passing:
flutter test
# If failing: Read error, understand root cause, fix code, re-run
```

### Level 3: Integration Test
```dart
// test/integration_test/app_test.dart
void main() {
  group('App Integration Tests', () {
    testWidgets('complete daily word test flow', (WidgetTester tester) async {
      await tester.pumpWidget(MyApp());
      
      // Navigate to word test
      await tester.tap(find.text('Start Daily Test'));
      await tester.pumpAndSettle();
      
      // Complete a word test
      await tester.tap(find.text('YES'));
      await tester.pumpAndSettle();
      
      // Verify progress updated
      expect(find.text('1/50'), findsOneWidget);
    });

    testWidgets('theme toggle works correctly', (WidgetTester tester) async {
      await tester.pumpWidget(MyApp());
      
      // Toggle theme
      await tester.tap(find.byType(ThemeToggle));
      await tester.pumpAndSettle();
      
      // Verify theme changed
      final darkTheme = tester.widget<MaterialApp>(find.byType(MaterialApp));
      expect(darkTheme.themeMode, equals(ThemeMode.dark));
    });
  });
}
```

```bash
# Start the app in debug mode
flutter run --debug

# Test key functionality manually:
# 1. Daily word generation (should show 50 words)
# 2. Yes/No buttons respond correctly  
# 3. Progress updates after each word
# 4. Theme toggle switches between light/dark
# 5. Progress screen shows statistics
# 6. App works offline after initial load

# Expected: Smooth user experience with no crashes
```

## Final validation Checklist
- [ ] All tests pass: `flutter test`
- [ ] No analysis errors: `flutter analyze`  
- [ ] No formatting issues: `dart format lib/ --set-exit-if-changed`
- [ ] App builds successfully: `flutter build apk --debug`
- [ ] Manual testing covers all user flows
- [ ] Dark/light theme toggle works smoothly
- [ ] Daily word selection algorithm prioritizes unknown words
- [ ] Progress tracking persists between app sessions
- [ ] Offline functionality works completely
- [ ] Performance is smooth on target devices (60fps)

---

## Anti-Patterns to Avoid
- ❌ Don't use StatefulWidget when Consumer/StateNotifier is more appropriate
- ❌ Don't perform database operations on the main thread without await
- ❌ Don't skip Hive adapter registration - will cause runtime crashes  
- ❌ Don't hardcode theme colors - use Theme.of(context) instead
- ❌ Don't ignore async/await patterns in Flutter - causes state issues
- ❌ Don't create new Random() instances repeatedly - use a singleton
- ❌ Don't forget to dispose controllers and close database connections
- ❌ Don't use setState() in StatelessWidget Consumer patterns

## Confidence Score: 9/10
This PRP provides comprehensive context for one-pass implementation including:
- ✅ Complete Flutter architecture guidance with official documentation
- ✅ Detailed state management patterns (Riverpod recommended)
- ✅ Specific database implementation strategy (Hive for simplicity)
- ✅ Real-world vocabulary dataset source (Google 10K words)
- ✅ Learning algorithm theory and implementation guidance
- ✅ Complete validation pipeline with executable commands
- ✅ Detailed gotchas and anti-patterns specific to Flutter
- ✅ Progressive task breakdown with clear dependencies

The only uncertainty is the exact spaced repetition algorithm tuning, but the Leitner system foundation provides a solid starting point that can be refined through user testing.