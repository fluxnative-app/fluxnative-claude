# React Native silent failures

Mistakes that compile, often look right on the web, and are wrong on a phone. Load this when
porting a mockup, HTML or a web design, and again before Verify.

Checked against Expo SDK 57 (`expo` 57.0, `react-native` 0.86.3, `react-native-web` 0.21,
`react-native-safe-area-context` 5.7, `@react-native/normalize-colors` 0.86.3). The "Seen in" line
names the installed file that shows the fact. On another version, re-check before you rely on it.

## 1. Rows stack as columns

- **What happens:** a View's children stack top to bottom. A mockup's horizontal group (icon,
  label, value) becomes a vertical pile.
- **Seen in:** Yoga `style/Style.h`, `flexDirection_ = FlexDirection::Column`.
- **Fix:** `flexDirection: 'row'` on every horizontal group, with `alignItems: 'center'` when the
  items should share a centre line.
- **Check:** for each row in the mockup, the matching View sets `flexDirection: 'row'`.

## 2. Long text pushes the row off screen

- **What happens:** children do not shrink by default, so a long name in a row pushes the price
  or chevron past the edge, or clips it.
- **Seen in:** Yoga `style/Style.h`, `DefaultFlexShrink = 0.0f` (the web default is 1).
- **Fix:** `flex: 1` (or `flexShrink: 1`) on the text column; `numberOfLines` on the label; keep
  the trailing value at its natural width.
- **Check:** render the row with the longest real string.

## 3. Text outside `<Text>`

- **What happens:** a bare string inside a View is reported as an error by a development build
  on a phone. On the web build it still shows, with only a console message, so a web preview
  hides it. A common source:
  `{count && <Badge />}` renders the string "0" when `count` is 0.
- **Seen in:** `react-native/Libraries/Renderer/implementations/ReactFabric-dev.js`, "Text strings
  must be rendered within a <Text> component."; `react-native-web/dist/cjs/exports/View/index.js`,
  a dev-only "Unexpected text node" message.
- **Fix:** every string inside `Text`; conditions as `count > 0 && …` or a ternary.
- **Check:** `rg -n "\{\s*[\w.]+\s*&&\s*<" app src -g '*.tsx'` and look at each left side that can
  be a number or an empty string. Read the browser console on the web build.

## 4. Safe areas twice, or not at all

- **What happens:** `SafeAreaView` from `react-native` is deprecated and is a plain View on
  Android, so content runs under the status bar there. Adding insets in a screen that already
  sits under a navigator header or tab bar pads the same edge twice.
- **Seen in:** `react-native/Libraries/Components/SafeAreaView/SafeAreaView.js` (`@deprecated`,
  `Platform.select({ios: …, default: View})`); `expo-router/build/ExpoRoot.js` wraps the app in
  `SafeAreaProvider`.
- **Fix:** `useSafeAreaInsets()` or `SafeAreaView` with `edges` from
  `react-native-safe-area-context`. Each edge has one owner: the header owns the top, the tab bar
  the bottom; a screen pads only an edge no bar covers (a full-screen modal, a screen with no
  header).
- **Check:** `rg -U -n "SafeAreaView[^;]*from ['\"]react-native['\"]" app src -g '*.tsx'`
  returns nothing; on a screen with a header and a tab bar, the first and last rows sit one
  spacing step from the bars, not two.

## 5. Android draws under the system bars

- **What happens:** apps built with this SDK target Android API 36. Android 16 always draws the
  app edge to edge, and Android 15 does unless the app opts out. Content goes under the status
  bar and the navigation bar. `expo-status-bar` has no background colour prop: it sets the icon
  style (`style`) and `hidden` only.
- **Seen in:** `react-native/gradle/libs.versions.toml` (`targetSdk = "36"`);
  `react-native/ReactAndroid/.../views/view/WindowUtil.kt` (`updateEdgeToEdgeFeatureFlag`);
  `expo-status-bar/build/types.d.ts` (`StatusBarProps`).
- **Fix:** apply safe-area insets on Android as on iOS (item 4). Let the header's ground run
  under the status bar; set the icon style with `expo-status-bar`.
- **Check:** an Android emulator or device shot, top and bottom. A web render cannot show this.

## 6. Shadows that only show on iOS

- **What happens:** `shadowOffset`, `shadowOpacity` and `shadowRadius` are typed iOS-only and do
  nothing on Android, which uses `elevation`. A card designed with a soft shadow is flat on
  Android, or looks different there when `elevation` stands in.
- **Seen in:** `react-native/Libraries/StyleSheet/StyleSheetTypes.d.ts`: `ShadowStyleIOS`;
  `elevation` marked `@platform android`; `boxShadow` on `ViewStyle`.
- **Fix:** separate with space between groups and a hairline between rows. Where a shadow is part of the
  design, use one `boxShadow` token from the theme and check it on an Android render.
- **Check:** `rg -n "shadowOffset|shadowRadius|elevation:" app src -g '*.tsx' -g '!**/theme/**'`
  returns nothing; shadows live in the token module.

## 7. Hover states that never fire

- **What happens:** a mockup's `:hover` style has no touch equivalent. `Pressable`'s style
  callback gets `pressed` only; `onHoverIn` needs a pointer, which a phone does not have.
- **Seen in:** `react-native/Libraries/Components/Pressable/Pressable.d.ts`,
  `PressableStateCallbackType { pressed }`.
- **Fix:** map hover to the pressed state: `style={({ pressed }) => [s.row, pressed && s.rowPressed]}`
  (or the platform ripple on Android). Every tappable element has visible press feedback.
- **Check:** `rg -n "onHoverIn|hovered" app src -g '*.tsx'` returns nothing outside web-only code.

## 8. Colours the parser drops

- **What happens:** a colour string React Native cannot parse becomes `undefined`, with no error:
  the background or text colour is simply missing. Some of these still show on the web build.
- **Seen in:** `react-native/Libraries/StyleSheet/processColor.js` returns `undefined` when
  `normalizeColor` fails. Tested against `@react-native/normalize-colors` 0.86.3:

| Accepted | Dropped |
|---|---|
| `#rgb`, `#rgba`, `#rrggbb`, `#rrggbbaa` | 5-digit hex such as `#abcde` |
| `rgb(0, 0, 0)`, `rgba(0,0,0,.5)`, `rgb(0 0 0 / 0.5)` | `rgb(0 0 0 / 50%)`, `rgb(100%, 0%, 0%)` |
| `hsl(200 50% 50%)`, `hsla(…)`, `hwb(…)` | `hsl(200deg, 50%, 50%)` |
| named colours, `transparent` | `oklch(…)`, `lab(…)`, `lch(…)`, `color-mix(…)` |
| | `var(--accent)`, `currentColor` (shown on the web build, dropped on a phone) |

- **Fix:** convert every design colour to hex in the token module.
- **Check:** `rg -n -i "\b(oklch|lab|lch|color-mix)\(|hsla?\(\s*\d[^)]*deg|var\(--|['\"]currentcolor" app src -g '*.ts' -g '*.tsx'`
  returns nothing.

## 9. CSS that React Native does not have

- **What happens:** web layout and units have no React Native meaning. TypeScript flags most of
  them; plain JavaScript drops them.
- **Seen in:** `StyleSheetTypes.d.ts`: `position` is `'absolute' | 'relative' | 'static'`;
  `display` is `'none' | 'flex' | 'contents'`; sizes are a number, `'auto'` or a percentage;
  `lineHeight` is a number of points.
- **Fix:**
  - `position: fixed` → place the element outside the `ScrollView`, or use the navigator's header
    or tab bar;
  - grid → rows of `flexDirection: 'row'` with `flexWrap` and `gap`, or `FlatList` with
    `numColumns`;
  - `vh`, `vw`, `rem`, `em` → numbers from the token module, or `useWindowDimensions()`;
  - `line-height: 1.5` → points (`lineHeight: 26` for a 17 pt body). `lineHeight: 1.5` is 1.5 pt
    and the lines overlap.
- **Check:** the type check passes; `rg -n "lineHeight:\s*[0-2](\.\d+)?\b" app src` returns nothing.

## 10. Text styles set on a View

- **What happens:** text does not inherit `color`, `fontSize` or `fontFamily` from a View, only
  from a parent `Text`. A wrapper styled like a CSS `div` leaves its text in the default style.
- **Seen in:** `StyleSheetTypes.d.ts`: `ViewStyle` has no `color` or `fontSize`; `TextStyle` does.
- **Fix:** put type roles on `Text` (or the project's text component), never on the wrapper.
- **Check:** the type check passes with the styles typed as `ViewStyle` and `TextStyle`.

## 11. Remote images with no size

- **What happens:** a remote image with no width and height (or `aspectRatio`) renders at zero
  size: the space is empty and nothing reports it.
- **Seen in:** `react-native/Libraries/Image/Image.ios.js` and `Image.android.js`: the size
  comes only from `source.width ?? props.width` (and height) and the style; nothing reads the
  downloaded file's size. `expo-image` not checked.
- **Fix:** give every remote image a width and an `aspectRatio` from the design, and a
  placeholder ground colour from the theme.
- **Check:** each `Image` with a `uri` source has a size or `aspectRatio` in its style.
