# Flutter 3.27.3 Migration Summary

This document summarizes the changes made to support Flutter 3.27.3 (Dart 3.6.1) for the `flutter_genui` package and related workspace packages.

## Version Constraints Updated

### Environment Constraints
All `pubspec.yaml` files were updated to support Flutter 3.27.3 and Dart 3.6.1:
- Dart SDK: Changed from `>=3.9.2` to `>=3.6.1 <4.0.0`
- Flutter SDK: Changed from `>=3.35.7` to `>=3.27.3 <4.0.0`

### Dependency Downgrades
Due to Flutter 3.27.3's bundled package versions, the following dependencies were downgraded to compatible versions:

| Package | Previous Version | New Version | Reason |
|---------|-----------------|-------------|--------|
| `dart_flutter_team_lints` | ^3.5.2 | ^3.2.0 | Requires Dart SDK >=3.7.0 |
| `lints` | ^6.0.0 | ^5.0.0 | Requires Dart SDK ^3.8.0 |
| `flutter_lints` | ^6.0.0 | ^5.0.0 | Requires Dart SDK >=3.8.0 |
| `test` | ^1.26.2 | ^1.25.8 | Requires test_api 0.7.6+ (SDK pins to 0.7.3) |
| `mockito` | ^5.5.0 | ^5.4.4 | Requires Dart SDK >=3.7.0 |
| `build_runner` | ^2.7.1 | ^2.4.13 | Requires Dart SDK >=3.7.0 |
| `gpt_markdown` | ^1.1.4 | ^1.0.9 | Requires Dart SDK >=3.7.0 |
| `process_runner` | ^4.2.3 | ^4.2.0 | Transitively requires newer test versions |

### SDK-Pinned Dependencies
The following dependencies were adjusted to match Flutter SDK pinned versions:
- `meta`: Changed from ^1.16.0 to ^1.15.0
- `file`: Changed from ^7.0.1 to ^7.0.0
- `process`: Changed from ^5.0.5 to ^5.0.2
- `collection`: Changed from ^1.19.1 to ^1.19.0
- `characters`: Changed from ^1.4.0 to ^1.3.0

## Code Changes

### Test Files
Fixed Dart 3.6 compilation errors where duplicate `_` parameters in the same scope are not allowed:

**Files modified:**
- `packages/flutter_genui/test/image_test.dart`
- `packages/flutter_genui/test/catalog_test.dart`

**Change:** Updated `buildChild: (_, [_])` to `buildChild: (_, [__])` to use distinct parameter names.

## Excluded Packages

### flutter_genui_a2ui
The `flutter_genui_a2ui` package and its example were temporarily excluded from the workspace because:
- It depends on the `a2a` package from git which requires Dart SDK >=3.9.0
- This is an optional integration package and not required for core `flutter_genui` functionality

**Workspace Changes:**
Commented out in root `pubspec.yaml`:
```yaml
# - packages/flutter_genui_a2ui  # Requires Dart SDK >=3.9.0
# - packages/flutter_genui_a2ui/example  # Requires Dart SDK >=3.9.0
```

## Packages Successfully Updated

The following packages are now fully compatible with Flutter 3.27.3:
- ✅ `flutter_genui` (main package)
- ✅ `flutter_genui_firebase_ai`
- ✅ `json_schema_builder`
- ✅ `examples/catalog_gallery`
- ✅ `examples/custom_backend`
- ✅ `examples/simple_chat`
- ✅ `examples/travel_app`
- ✅ `tool/fix_copyright`
- ✅ `tool/test_and_fix`

## Test Results

All tests in the `flutter_genui` package pass successfully:
- **Total Tests:** 106
- **Passed:** 106 ✅
- **Failed:** 0

## Notes

1. Some packages have newer versions available that are incompatible with Dart 3.6.1. This is expected and documented in the `flutter pub get` output.

2. To restore `flutter_genui_a2ui` functionality, either:
   - Wait for the `a2a` package to support Dart 3.6.1
   - Upgrade to a newer Flutter version that includes Dart 3.9.0+
   - Fork and modify the `a2a` dependency to support Dart 3.6.1

3. All changes maintain backward compatibility with the existing API.

## Verification

To verify the migration:
```bash
cd /path/to/genui-3.27.3
flutter --version  # Should show Flutter 3.27.3 with Dart 3.6.1
flutter pub get    # Should succeed
cd packages/flutter_genui
flutter test       # All 106 tests should pass
```

