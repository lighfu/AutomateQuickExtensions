# AutomateQuickExtensions
Automate向け 拡張機能アプリケーション
Extension Applications for Automate

## 概要
自動化アプリである、Automate向けに作成された拡張機能アプリケーションです。
Broadcast, StartActivity, StartService などを活用して、拡張機能にアクセスできます。

## Version history
### t1.0a
- Worked on Android 11, 10, 9, 8, 7
- It also works on Android 5.1, but text updates behave suspiciously.
- Changed the behavior of each version.
- Error: App crashes when the last overlay text is stopped

## 機能
- FullWebview (GUIの提供, HTML) `[in preparation]`
- TextOverlay (Overlay表示) `[Public]`
- ... 今後追加するかも。

現時点では、まだAlphaバージョンのため、バグやクラッシュを引き起こすかもしれません。
もしよければ、バグやクラッシュが発生するまでの行動や、Android のバージョンを、
https://llamalab.com/automate/community/flows/40250 で報告するか、問題が明らかになっている場合には、
Github の Issues で報告してもらえると助かります。

# Bug-Todo
- TextOverlay の終了コードでエラーが発生する
