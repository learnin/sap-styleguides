> Translated from [English original on 17.6.2020](https://github.com/SAP/styleguides/tree/012d2e8bdc19de321ed51c1a2310dd07e4f87de3).
> Latest version [in English](CleanABAP.md).

# Clean ABAP

> [**日本語**](CleanABAP_ja.md) > &nbsp;·&nbsp; > [English](CleanABAP.md) > &nbsp;·&nbsp; > [中文](CleanABAP_zh.md) > &nbsp;·&nbsp; > [Français](CleanABAP_fr.md) > &nbsp;·&nbsp; > [Deutsch](CleanABAP_de.md)

このガイドは [ABAP](https://en.wikipedia.org/wiki/ABAP) 向けに [Robert C. Martin の _Clean Code_] を採用したものです。

[チートシート](cheat-sheet/CheatSheet.md) は印刷に最適化されたものです。

[robert c. martin の _clean code_]: https://www.oreilly.com/library/view/clean-code/9780136083238/

## 目次

- [やり方](#やり方)
  - [クリーンコードを始めるには](#クリーンコードを始めるには)
  - [レガシーコードをリファクタするには](#レガシーコードをリファクタするには)
  - [自動的にチェックするには](#自動的にチェックするには)
  - [他のガイドとの関係性](#他のガイドとの関係性)
  - [反対するには](#反対するには)
- [命名](#命名)
  - [意味のある名前を使う](#意味のある名前を使う)
  - [ソリューションドメインと問題ドメインの用語を選ぶ](#ソリューションドメインと問題ドメインの用語を選ぶ)
  - [複数形を使う](#複数形を使う)
  - [発音可能な名前を使う](#発音可能な名前を使う)
  - [略語を避ける](#略語を避ける)
  - [どこでも同じ略語を使う](#どこでも同じ略語を使う)
  - [クラスには名詞を、メソッドには動詞を使う](#クラスには名詞を、メソッドには動詞を使う)
  - ["data", "info", "object" などの意味のない言葉を避ける](#data-info-object-などの意味のない言葉を避ける)
  - [1つの概念には1つの言葉を使う](#1つの概念には1つの言葉を使う)
  - [パターン名はそれを意図する場合にのみ使う](#パターン名はそれを意図する場合にのみ使う)
  - [エンコーディング、特にハンガリアン記法と接頭辞を避ける](#エンコーディング、特にハンガリアン記法と接頭辞を避ける)
- [言語](#言語)
  - [古いABAPリリースに注意する](#古いABAPリリースに注意する)
  - [パフォーマンスに注意する](#パフォーマンスに注意する)
  - [手続き型プログラミングよりもオブジェクト指向を選ぶ](#手続き型プログラミングよりもオブジェクト指向を選ぶ)
  - [手続き型の言語構造よりも関数型の言語構造を選ぶ](#手続き型の言語構造よりも関数型の言語構造を選ぶ)
  - [廃止された言語要素を避ける](#廃止された言語要素を避ける)
  - [デザインパターンを賢く使う](#デザインパターンを賢く使う)
- [定数](#定数)
  - [マジックナンバーの代わりに定数を使う](#マジックナンバーの代わりに定数を使う)
  - [定数インターフェースよりも列挙クラスを選ぶ](#定数インターフェースよりも列挙クラスを選ぶ)
  - [列挙クラスを使用しない場合は定数をグループ化する](#列挙クラスを使用しない場合は定数をグループ化する)
- [変数](#変数)
  - [事前宣言よりもインライン宣言を選ぶ](#事前宣言よりもインライン宣言を選ぶ)
  - [選択の分岐内でインライン宣言をしない](#選択の分岐内でインライン宣言をしない)
  - [事前宣言を連結させない](#事前宣言を連結させない)
  - [FIELD-SYMBOLよりもREF TOを選択する](#FIELD-SYMBOLよりもREF-TOを選択する)
- [テーブル](#テーブル)
  - [正しいテーブルデータ型を使う](#正しいテーブルデータ型を使う)
  - [DEFAULT KEY を避ける](#DEFAULT-KEY-を避ける)
  - [APPEND TO よりも INSERT INTO TABLE を選ぶ](#APPEND-TO-よりも-INSERT-INTO-TABLE-を選ぶ)
  - [READ TABLE や LOOP AT よりも LINE_EXISTS を選ぶ](#READ-TABLE-や-LOOP-AT-よりも-LINE_EXISTS-を選ぶ)
  - [LOOP AT よりも READ TABLE を選ぶ](#LOOP-AT-よりも-READ-TABLE-を選ぶ)
  - [ネストした IF よりも LOOP AT WHERE を選ぶ](#ネストした-IF-よりも-LOOP-AT-WHERE-を選ぶ)
  - [不要なテーブルの読み取りを避ける](#不要なテーブルの読み取りを避ける)
- [Strings](#strings)
  - [リテラルを定義するには ` を使う](#リテラルを定義するには--を使う)
  - [テキストを組み立てるには | を使う](#テキストを組み立てるには--を使う)
- [Booleans](#booleans)
  - [Booleansを賢く使う](#Booleansを賢く使う)
  - [BooleanにはABAP_BOOLを使う](#BooleanにはABAP_BOOLを使う)
  - [比較にはABAP_TRUEとABAP_FALSEを使う](#比較にはABAP_TRUEとABAP_FALSEを使う)
  - [Boolean変数をセットするにはXSDBOOLを使う](#Boolean変数をセットするにはXSDBOOLを使う)
- [条件](#条件)
  - [条件を肯定にしてみる](#条件を肯定にしてみる)
  - [NOT ISよりもIS NOTを選ぶ](#NOT-ISよりもIS-NOTを選ぶ)
  - [複素条件を分解することを考える](#複素条件を分解することを考える)
  - [複雑な条件を抽出することを考える](#複雑な条件を抽出することを考える)
- [If](#if)
  - [空のIF分岐を作らない](#空のIF分岐を作らない)
  - [複数の択一条件にはELSE IFよりもCASEを選ぶ](#複数の択一条件にはELSE-IFよりもCASEを選ぶ)
  - [ネストの深さを浅くする](#ネストの深さを浅くする)
- [正規表現](#正規表現)
  - [正規表現よりもシンプルなメソッドを選ぶ](#正規表現よりもシンプルなメソッドを選ぶ)
  - [正規表現よりも基本的なチェックを選ぶ](#正規表現よりも基本的なチェックを選ぶ)
  - [複雑な正規表現は組み立てることを考える](#複雑な正規表現は組み立てることを考える)
- [クラス](#クラス)
  - [クラス: オブジェクト指向](#クラス-オブジェクト指向)
    - [静的クラスよりもオブジェクトを選ぶ](#静的クラスよりもオブジェクトを選ぶ)
    - [継承よりもコンポジションを選ぶ](#継承よりもコンポジションを選ぶ)
    - [同じクラスにステートフルとステートレスを混在させない](#同じクラスにステートフルとステートレスを混在させない)
  - [スコープ](#スコープ)
    - [デフォルトではグローバル、必要に応じてローカルのみ](#デフォルトではグローバル、必要に応じてローカルのみ)
    - [継承を意図しない場合はFINALにする](#継承を意図しない場合はFINALにする)
    - [Members PRIVATE by default, PROTECTED only if needed](#members-private-by-default-protected-only-if-needed)
    - [Consider using immutable instead of getter](#consider-using-immutable-instead-of-getter)
    - [Use READ-ONLY sparingly](#use-read-only-sparingly)
  - [Constructors](#constructors)
    - [Prefer NEW to CREATE OBJECT](#prefer-new-to-create-object)
    - [If your global class is CREATE PRIVATE, leave the CONSTRUCTOR public](#if-your-global-class-is-create-private-leave-the-constructor-public)
    - [Prefer multiple static creation methods to optional parameters](#prefer-multiple-static-creation-methods-to-optional-parameters)
    - [Use descriptive names for multiple creation methods](#use-descriptive-names-for-multiple-creation-methods)
    - [Make singletons only where multiple instances don't make sense](#make-singletons-only-where-multiple-instances-dont-make-sense)
- [Methods](#methods)
  - [Calls](#calls)
    - [Prefer functional to procedural calls](#prefer-functional-to-procedural-calls)
    - [Omit RECEIVING](#omit-receiving)
    - [Omit the optional keyword EXPORTING](#omit-the-optional-keyword-exporting)
    - [Omit the parameter name in single parameter calls](#omit-the-parameter-name-in-single-parameter-calls)
    - [Omit the self-reference me when calling an instance method](#omit-the-self-reference-me-when-calling-an-instance-method)
  - [Methods: Object orientation](#methods-object-orientation)
    - [Prefer instance to static methods](#prefer-instance-to-static-methods)
    - [パブリックインスタンスメソッドはインタフェースの一部でなければならない](#パブリックインスタンスメソッドはインタフェースの一部でなければならない)
  - [Parameter Number](#parameter-number)
    - [Aim for few IMPORTING parameters, at best less than three](#aim-for-few-importing-parameters-at-best-less-than-three)
    - [Split methods instead of adding OPTIONAL parameters](#split-methods-instead-of-adding-optional-parameters)
    - [Use PREFERRED PARAMETER sparingly](#use-preferred-parameter-sparingly)
    - [RETURN, EXPORT, or CHANGE exactly one parameter](#return-export-or-change-exactly-one-parameter)
  - [Parameter Types](#parameter-types)
    - [Prefer RETURNING to EXPORTING](#prefer-returning-to-exporting)
    - [RETURNING large tables is usually okay](#returning-large-tables-is-usually-okay)
    - [Use either RETURNING or EXPORTING or CHANGING, but not a combination](#use-either-returning-or-exporting-or-changing-but-not-a-combination)
    - [Use CHANGING sparingly, where suited](#use-changing-sparingly-where-suited)
    - [boolean型の入力パラメータの代わりにメソッドを分割する](#boolean型の入力パラメータの代わりにメソッドを分割する)
  - [Parameter Names](#parameter-names)
    - [Consider calling the RETURNING parameter RESULT](#consider-calling-the-returning-parameter-result)
  - [Parameter Initialization](#parameter-initialization)
    - [Clear or overwrite EXPORTING reference parameters](#clear-or-overwrite-exporting-reference-parameters)
      - [Take care if input and output could be the same](#take-care-if-input-and-output-could-be-the-same)
    - [Don't clear VALUE parameters](#dont-clear-value-parameters)
  - [Method Body](#method-body)
    - [Do one thing, do it well, do it only](#do-one-thing-do-it-well-do-it-only)
    - [正常系かエラー処理に集中する, 両方ではなく](#正常系かエラー処理に集中する-両方ではなく)
    - [Descend one level of abstraction](#descend-one-level-of-abstraction)
    - [Keep methods small](#keep-methods-small)
  - [Control flow](#control-flow)
    - [Fail fast](#fail-fast)
    - [CHECK vs. RETURN](#check-vs-return)
    - [Avoid CHECK in other positions](#avoid-check-in-other-positions)
- [Error Handling](#error-handling)
  - [Messages](#messages)
    - [Make messages easy to find](#make-messages-easy-to-find)
  - [Return Codes](#return-codes)
    - [Prefer exceptions to return codes](#prefer-exceptions-to-return-codes)
    - [Don't let failures slip through](#dont-let-failures-slip-through)
  - [Exceptions](#exceptions)
    - [Exceptions are for errors, not for regular cases](#exceptions-are-for-errors-not-for-regular-cases)
    - [Use class-based exceptions](#use-class-based-exceptions)
  - [Throwing](#throwing)
    - [Use own super classes](#use-own-super-classes)
    - [Throw one type of exception](#throw-one-type-of-exception)
    - [Use sub-classes to enable callers to distinguish error situations](#use-sub-classes-to-enable-callers-to-distinguish-error-situations)
    - [Throw CX_STATIC_CHECK for manageable exceptions](#throw-cx_static_check-for-manageable-exceptions)
    - [Throw CX_NO_CHECK for usually unrecoverable situations](#throw-cx_no_check-for-usually-unrecoverable-situations)
    - [Consider CX_DYNAMIC_CHECK for avoidable exceptions](#consider-cx_dynamic_check-for-avoidable-exceptions)
    - [Dump for totally unrecoverable situations](#dump-for-totally-unrecoverable-situations)
    - [Prefer RAISE EXCEPTION NEW to RAISE EXCEPTION TYPE](#prefer-raise-exception-new-to-raise-exception-type)
  - [Catching](#catching)
    - [Wrap foreign exceptions instead of letting them invade your code](#wrap-foreign-exceptions-instead-of-letting-them-invade-your-code)
- [Comments](#comments)
  - [Express yourself in code, not in comments](#express-yourself-in-code-not-in-comments)
  - [不適切な命名をコメントで補おうとしないでください](#不適切な命名をコメントで補おうとしないでください)
  - [Use methods instead of comments to segment your code](#use-methods-instead-of-comments-to-segment-your-code)
  - [Write comments to explain the why, not the what](#write-comments-to-explain-the-why-not-the-what)
  - [Design goes into the design documents, not the code](#design-goes-into-the-design-documents-not-the-code)
  - [Comment with ", not with \*](#comment-with--not-with-)
  - [Put comments before the statement they relate to](#put-comments-before-the-statement-they-relate-to)
  - [Delete code instead of commenting it](#delete-code-instead-of-commenting-it)
  - [Use FIXME, TODO, and XXX and add your ID](#use-fixme-todo-and-xxx-and-add-your-id)
  - [Don't add method signature and end-of comments](#dont-add-method-signature-and-end-of-comments)
  - [Don't duplicate message texts as comments](#dont-duplicate-message-texts-as-comments)
  - [ABAP Doc only for public APIs](#abap-doc-only-for-public-apis)
  - [Prefer pragmas to pseudo comments](#prefer-pragmas-to-pseudo-comments)
- [Formatting](#formatting)
  - [Be consistent](#be-consistent)
  - [Optimize for reading, not for writing](#optimize-for-reading-not-for-writing)
  - [Use the Pretty Printer before activating](#use-the-pretty-printer-before-activating)
  - [Use your Pretty Printer team settings](#use-your-pretty-printer-team-settings)
  - [No more than one statement per line](#no-more-than-one-statement-per-line)
  - [Stick to a reasonable line length](#stick-to-a-reasonable-line-length)
  - [Condense your code](#condense-your-code)
  - [Add a single blank line to separate things, but not more](#add-a-single-blank-line-to-separate-things-but-not-more)
  - [Don't obsess with separating blank lines](#dont-obsess-with-separating-blank-lines)
  - [Align assignments to the same object, but not to different ones](#align-assignments-to-the-same-object-but-not-to-different-ones)
  - [Close brackets at line end](#close-brackets-at-line-end)
  - [Keep single parameter calls on one line](#keep-single-parameter-calls-on-one-line)
  - [Keep parameters behind the call](#keep-parameters-behind-the-call)
  - [If you break, indent parameters under the call](#if-you-break-indent-parameters-under-the-call)
  - [Line-break multiple parameters](#line-break-multiple-parameters)
  - [Align parameters](#align-parameters)
  - [Break the call to a new line if the line gets too long](#break-the-call-to-a-new-line-if-the-line-gets-too-long)
  - [Indent and snap to tab](#indent-and-snap-to-tab)
  - [Indent in-line declarations like method calls](#indent-in-line-declarations-like-method-calls)
  - [型句の位置を揃えない](#型句の位置を揃えない)
- [Testing](#testing)
  - [Principles](#principles)
    - [Write testable code](#write-testable-code)
    - [Enable others to mock you](#enable-others-to-mock-you)
    - [Readability rules](#readability-rules)
    - [Don't make copies or write test reports](#dont-make-copies-or-write-test-reports)
    - [Test publics, not private internals](#test-publics-not-private-internals)
    - [Don't obsess about coverage](#dont-obsess-about-coverage)
  - [Test Classes](#test-classes)
    - [Call local test classes by their purpose](#call-local-test-classes-by-their-purpose)
    - [Put tests in local classes](#put-tests-in-local-classes)
    - [Put help methods in help classes](#put-help-methods-in-help-classes)
    - [How to execute test classes](#how-to-execute-test-classes)
  - [Code Under Test](#code-under-test)
    - [Name the code under test meaningfully, or default to CUT](#name-the-code-under-test-meaningfully-or-default-to-cut)
    - [Test against interfaces, not implementations](#test-against-interfaces-not-implementations)
    - [Extract the call to the code under test to its own method](#extract-the-call-to-the-code-under-test-to-its-own-method)
  - [Injection](#injection)
    - [Use dependency inversion to inject test doubles](#use-dependency-inversion-to-inject-test-doubles)
    - [Consider to use the tool ABAP test double](#consider-to-use-the-tool-abap-test-double)
    - [Exploit the test tools](#exploit-the-test-tools)
    - [Use test seams as temporary workaround](#use-test-seams-as-temporary-workaround)
    - [Use LOCAL FRIENDS to access the dependency-inverting constructor](#use-local-friends-to-access-the-dependency-inverting-constructor)
    - [Don't misuse LOCAL FRIENDS to invade the tested code](#dont-misuse-local-friends-to-invade-the-tested-code)
    - [Don't change the productive code to make the code testable](#dont-change-the-productive-code-to-make-the-code-testable)
    - [Don't sub-class to mock methods](#dont-sub-class-to-mock-methods)
    - [Don't mock stuff that's not needed](#dont-mock-stuff-thats-not-needed)
    - [Don't build test frameworks](#dont-build-test-frameworks)
  - [Test Methods](#test-methods)
    - [Test method names: reflect what's given and expected](#test-method-names-reflect-whats-given-and-expected)
    - [Use given-when-then](#use-given-when-then)
    - ["When" is exactly one call](#when-is-exactly-one-call)
    - [Don't add a TEARDOWN unless you really need it](#dont-add-a-teardown-unless-you-really-need-it)
  - [Test Data](#test-data)
    - [Make it easy to spot meaning](#make-it-easy-to-spot-meaning)
    - [Make it easy to spot differences](#make-it-easy-to-spot-differences)
    - [Use constants to describe purpose and importance of test data](#use-constants-to-describe-purpose-and-importance-of-test-data)
  - [Assertions](#assertions)
    - [Few, focused assertions](#few-focused-assertions)
    - [Use the right assert type](#use-the-right-assert-type)
    - [Assert content, not quantity](#assert-content-not-quantity)
    - [Assert quality, not content](#assert-quality-not-content)
    - [Use FAIL to check for expected exceptions](#use-fail-to-check-for-expected-exceptions)
    - [Forward unexpected exceptions instead of catching and failing](#forward-unexpected-exceptions-instead-of-catching-and-failing)
    - [Write custom asserts to shorten code and avoid duplication](#write-custom-asserts-to-shorten-code-and-avoid-duplication)

## やり方

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#やり方)

### クリーンコードを始めるには

> [Clean ABAP](#clean-abap) > [目次](#目次) > [やり方](#やり方) > [本節](#クリーンコードを始めるには)

Clean Code を初めて利用する場合は、まず、[Robert C. Martin の _Clean Code_] を読んでください。
[Clean Code Developer initiative](https://clean-code-developer.com/) は一般的にトピックを段階的にスムーズに導入し始めるのに役立つでしょう。

[Booleans](#booleans) や [Conditions](#conditions) 、 [Ifs](#ifs) のようにわかりやすく、広く受け入れられるものから始めることをお勧めします。

おそらく [メソッド](#メソッド) 、特に [Do one thing, do it well, do it only](#do-one-thing-do-it-well-do-it-only) と [Small](#keep-methods-small) の節がもっとも有益でしょう。なぜなら、これらはコードの全体的な構造を大幅に改善するからです。

ここに書かれているトピックの中には、経験は豊富だがクリーンコードに慣れていないチームでは難しい議論を引き起こす可能性があるものもあります。これらのトピックは完全に「健全」ですが、最初は慣れるのが難しいかもしれません。

特に [コメント](#コメント)、 [命名](#命名)、 [フォーマット](#フォーマット) は、宗教的な論争に発展する可能性があるので、クリーンコードの効果をすでに経験しているチームにのみ適用してください。

### レガシーコードをリファクタするには

> [Clean ABAP](#clean-abap) > [目次](#目次) > [やり方](#やり方) > [本節](#レガシーコードをリファクタするには)

[Booleans](#booleans)、[Conditions](#conditions)、[Ifs](#ifs)、[メソッド](#メソッド) は、コンフリクトなしに新しいコードを適用できるため、変更できない、または変更したくないコードが大量にあるレガシープロジェクトで作業している場合に、最も有益なトピックです。

[命名](#命名) は、古いコードと新しいコードの間に [エンコーディング、特にハンガリアン記法と接頭辞を避ける](#エンコーディング、特にハンガリアン記法と接頭辞を避ける) のような節を無視した方がよいほどまでの断絶を引き起こす可能性があるため、レガシープロジェクトには非常に厳しいトピックです。

リファクタリングを行う際には、同じ開発オブジェクト内で異なる開発スタイルを混在させないようにしてください。レガシーコードに事前宣言しか含まれておらず、インライン宣言を使用するように完全にリファクタリングすることが不可能な場合、2 つのスタイルを混合するよりもレガシースタイルを続ける方が良いでしょう。スタイルを混合することで混乱を招く同様の状況がいくつかあります。例えば、

- ループ時に `REF TO` と `FIELD-SYMBOL` を混在させる
- `CONSTRUCTOR` 呼び出し時に `NEW` と `CREATE OBJECT` を混在させる
- 1 つのパラメータしか返さないメソッドのシグニチャで `RETURNING` と `EXPORTING` を混在させる

リファクタリングの 4 段階の計画で良好な結果が得られました:

1. チームに参加してもらう。新しいスタイルを伝えて説明し、プロジェクトチームの全員に同意してもらいましょう。一度にすべてのガイドラインにコミットする必要はなく、議論の余地のない小さなサブセットから始めて、そこから進化させてください。

2. ボーイスカウトのルールに従ってください：編集したコードは、常に見つけたときよりも少しきれいな状態にしておきましょう。「キャンプ場の掃除」に何時間もかけて執着するのではなく、数分余分に費やして、時間の経過とともに改善がどのように蓄積されていくかを観察してください。

3. クリーンな島を作る: 時々、小さなオブジェクトやコンポーネントを選んで、あらゆる面でクリーンにします。これらの島は、あなたがやっていることの利点を示し、さらなるリファクタリングのためのしっかりとテストされたホームベースを形成します。

4. それについて話してください。古風な[Fagan コードレビュー](https://en.wikipedia.org/wiki/Fagan_inspection)を設定したり、情報セッションを開催したり、お気に入りのチャットツールでディスカッションボードを形成するかどうかに関わらず、チームが共通の理解を成長させることができるように、あなたの経験や学習について話す必要があります。

### 自動的にチェックするには

> [Clean ABAP](#clean-abap) > [目次](#目次) > [やり方](#やり方) > [本節](#自動的にチェックするには)

ここで説明するアンチパターンを自動的に検出する静的コードチェックの包括的なスイートはありません。

ABAP テストコックピット、コードインスペクタ、拡張チェック、チェックマンは、特定の問題を見つけるのに役立ついくつかのチェックを提供しています。

コードインスペクタチェックのオープンソースのコレクションである [abapOpenChecks](https://github.com/larshp/abapOpenChecks) も記述されているアンチパターンの一部をカバーしています。

[abaplint](https://github.com/abaplint/abaplint) はオープンソースの ABAP パーサの再実装です。SAP システムなしで動作し、abapGit でシリアライズされたコード上で使用されるように設計されています。複数の統合機能（GitHub Actions、Jenkins、テキストエディタなど）を提供し、いくつかのアンチパターンをカバーし、フォーマットやコード規約のチェックにも使用できます。

### 他のガイドとの関係性

> [Clean ABAP](#clean-abap) > [目次](#目次) > [やり方](#やり方) > [本節](#他のガイドとの関係性)

私たちのガイドは、クリーンコードの _精神_ に従っていて、例えば、[管理可能な例外のための CX_STATIC_CHECK のスロー](#管理可能な例外のための CX_STATIC_CHECK のスロー)など、ABAP プログラミング言語にいくつかの調整をしたことを意味します。

いくつかの要素は [ABAP プログラミングガイドライン](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenabap_pgl.htm) から引用されており、このガイドはガイドラインとほとんど互換性があります。逸脱は示され、常によりクリーンなコードの精神に基づいています。

このガイドでは、[ABAP 開発に関する DSAG の推奨事項](https://www.dsag.de/sites/default/files/dsag_recommendation_abap_development.pdf) も尊重していますが、ほとんどの詳細についてはより正確な情報を提供しています。

発行以来、Clean ABAP は、S/4HANA に取り組む数百人の開発者を含む多くの SAP 社内開発チームのリファレンスガイドとなっています。

### 反対するには

> [Clean ABAP](#clean-abap) > [目次](#目次) > [やり方](#やり方) > [本節](#反対するには)

このスタイルガイドは、すでに Clean Code を知っている方や、これから Clean Code に取り組む方に向けて、Clean Code を _特に ABAP に_ どのように適用するかに重点を置いて書きました。

そのため、原書や関連資料と同じ長さと深さですべての概念を紹介しているわけではないことに留意ください。それらは、特に私たちがうまく説明していないからといって、ここにあることに反対する場合には、読む価値があります。節の中のリンクを使って、私たちのガイダンスの背景を読んでみてください。

ここで書かれていることについて、議論したり、反対したりすることは自由です。クリーンコードの柱の一つは、 _チームのルール_ です。ただ、捨てる前に公平なチャンスを与えることを忘れないでください。

[CONTRIBUTING.md](../CONTRIBUTING.md) には、このガイドを変更したり、細かい部分で逸脱したりする方法についての提案が書かれています。

## 命名

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#命名)

### 意味のある名前を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#意味のある名前を使う)

物事の内容や意味が伝わる名前を使いましょう。

```ABAP
CONSTANTS max_wait_time_in_seconds TYPE i ...
DATA customizing_entries TYPE STANDARD TABLE ...
METHODS read_user_preferences ...
CLASS /clean/user_preference_reader ...
```

データ型や技術的なエンコーディングに注目してはいけません。これらはコードを理解することにはほとんど貢献しません。

```ABAP
" アンチパターン
CONSTANTS sysubrc_04 TYPE sysubrc ...
DATA iso3166tab TYPE STANDARD TABLE ...
METHODS read_t005 ...
CLASS /dirty/t005_reader ...
```

[不適切な命名をコメントで補おうとしないでください](#不適切な命名をコメントで補おうとしないでください)

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Use Intention-Revealing Names_ を参照してください。

### ソリューションドメインと問題ドメインの用語を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#ソリューションドメインと問題ドメインの用語を選ぶ)

ソリューションドメイン、すなわち「キュー」や「ツリー」などのコンピュータサイエンス用語と、問題ドメイン、すなわち「勘定科目」や「元帳」などのビジネス分野の用語で、良い名前を検索します。

ビジネスライクなレイヤーには、問題のドメインにちなんだ名前をつけるのが最適です。これは特に、API やビジネスオブジェクトなど、ドメイン駆動設計で設計されたコンポーネントに当てはまります。

ファクトリクラスや抽象アルゴリズムなどの主に技術的な機能を提供するレイヤーには、ソリューションのドメインにちなんだ名前をつけると最もよく聞こえます。

いずれにしても、独自の言葉を作ろうとしないでください。開発者、プロダクトオーナー、パートナー、顧客の間で情報を交換できるようにする必要があるので、カスタマイズされた辞書を使わずに、これらすべてに関連していると思われる名前を選びましょう。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Use Solution Domain Names_ と _Use Problem Domain Names_ を参照してください。

### 複数形を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#複数形を使う)

SAP では、テーブルの名前を単数形、例えば「国のテーブル」の場合は `country` というように命名するという時代遅れの慣習があります。しかし、SAP 以外の世界では一般的な傾向として、リストには複数形を使用します。そのため、代わりに `countries` を使用することをお勧めします。

> このアドバイスは、主に変数やクラスの属性などを対象としています。開発オブジェクトの場合は、例えば、データベースのテーブル（「透過テーブル」）を単数形で命名するために広く使われている規約など、競合するパターンの方が適切かもしれません。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Use Intention-Revealing Names_ を参照してください。

### 発音可能な名前を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#発音可能な名前を使う)

私たちは、対象についてたくさん考えたり、話したりしています。そのため、発音できる名前を使いましょう。例えば、 `dobjt` のような暗号的なものではなく、 `detection_object_types` を使用してください。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Use Pronounceable Names_ を参照してください。

### 略語を避ける

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#略語を避ける)

スペースに余裕がある場合は、名前を完全に書き出してください。長さの制限を超える場合のみ省略してください。

どうしても省略しなければならない場合は、 _重要でない_ 単語から始めましょう。

言葉を略すことは一見効率的に見えても、すぐに誤解を招くことになります。例えば、 `cust` の「cust」が「カスタマイズ」を指しているのか、「顧客」を指しているのか、「カスタム」を指しているのかは明確ではありません。この 3 つの用語はいずれも SAP アプリケーションでは一般的なものです。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Make Meaningful Distinctions_ を参照してください。

### どこでも同じ略語を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#どこでも同じ略語を使う)

人々は関連するコードを見つけるためにキーワードを検索します。これをサポートするために、同じものを同じ略語で表現します。例えば、"detection object type" を常に "dobjt" と略すようにし、 "dot" や "dotype"、"detobjtype" などを混在させないようにしてください。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Use Searchable Names_ を参照してください。

### クラスには名詞を、メソッドには動詞を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#クラスには名詞を、メソッドには動詞を使う)

クラス、インターフェース、オブジェクトの名前には名詞や名詞句を使用しましょう：

```ABAP
CLASS /clean/account
CLASS /clean/user_preferences
INTERFACE /clean/customizing_reader
```

メソッドの名前には動詞や動詞句を使用しましょう：

```ABAP
METHODS withdraw
METHODS add_message
METHODS read_entries
```

Boolean 型メソッドの名前を `is_` や `has_` のような動詞で開始すると、読みやすくなります：

```ABAP
IF is_empty( table ).
```

関数にはメソッドのような名前をつけることをお勧めします：

```ABAP
FUNCTION /clean/read_alerts
```

### "data", "info", "object" などの意味のない言葉を避ける

>[Cle-ABAP-clean-bap) > [目次](#目次) > [命名](#命名) > [本節](#data-info-object-などの意味のない言葉を避ける)

意味のない言葉は省略しましょう

```ABAP
account  " account_data とするのではなく
alert    " alert_object とするのではなく
```

または、より具体的に意味のある言葉に置き換えてください

```ABAP
user_preferences          " user_info とするのではなく
response_time_in_seconds  " response_time_variable とするのではなく
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Make Meaningful Distinctions_ を参照してください。

### 1つの概念には1つの言葉を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#1つの概念には1つの言葉を使う)

```ABAP
METHODS read_this.
METHODS read_that.
METHODS read_those.
```

概念を表す言葉を選び、それに拘ります。他の同義語を混在させないようにしてください。同義語があると、読者はありもしない意味の違いを見つけようとして時間を浪費してしまいます。

```ABAP
" アンチパターン
METHODS read_this.
METHODS retrieve_that.
METHODS query_those.
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Pick One Word per Concept_ を参照してください。

### パターン名はそれを意図する場合にのみ使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#パターン名はそれを意図する場合にのみ使う)

本当にそれを意図しているのでなければ、クラスやインターフェースにソフトウェアデザインパターンの名前を使わないようにしましょう。例えば、本当にファクトリーデザインパターンを実装していない限り、クラス名を `file_factory` とはしないでください。最も一般的なパターンには、
[singleton](https://en.wikipedia.org/wiki/Singleton_pattern)、
[factory](https://en.wikipedia.org/wiki/Factory_method_pattern)、
[facade](https://en.wikipedia.org/wiki/Facade_pattern)、
[composite](https://en.wikipedia.org/wiki/Composite_pattern)、
[decorator](https://en.wikipedia.org/wiki/Decorator_pattern)、
[iterator](https://en.wikipedia.org/wiki/Iterator_pattern)、
[observer](https://en.wikipedia.org/wiki/Observer_pattern)、
[strategy](https://en.wikipedia.org/wiki/Strategy_pattern)
などがあります。

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 2: Meaningful Names: Avoid Disinformation_ を参照してください。

### エンコーディング、特にハンガリアン記法と接頭辞を避ける

> [Clean ABAP](#clean-abap) > [目次](#目次) > [命名](#命名) > [本節](#エンコーディング、特にハンガリアン記法と接頭辞を避ける)

私たちは、 _すべての_ エンコーディング接頭辞を取り除くことをお勧めします。

```ABAP
METHOD add_two_numbers.
  result = a + b.
ENDMETHOD.
```

次のように、不必要に長くするのではなく

```ABAP
METHOD add_two_numbers.
  rv_result = iv_a + iv_b.
ENDMETHOD.
```

> 理由は [Avoid Encodings](sub-sections/AvoidEncodings.md) に詳しく書かれています。

## 言語

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#言語)

### 古いABAPリリースに注意する

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#古いABAPリリースに注意する)

古いABAPリリースでコーディングを行う場合は、このガイドのアドバイスに注意してください。以下の推奨事項の多くは、古いABAPリリースではサポートされていない可能性のある比較的新しい文法や構文を使用しています。サポートしなければならない最古のリリースで適用したいガイドラインを検証してください。ただし、クリーンコード全体を単純に破棄しないでください。ほとんどのルール（名前付けやコメントなど）は _どの_ ABAP バージョンでも動作します。

### パフォーマンスに注意する

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#パフォーマンスに注意する)

高いパフォーマンスが要求されるコンポーネントをコーディングする場合は、このガイドのアドバイスに注意してください。クリーンコードのいくつかの側面は、パフォーマンスを低下させたり（メソッド呼び出しが多くなる）、メモリをより消費したり（オブジェクトが多くなる）する可能性があります。ABAPには、これを強めてしまう可能性のあるいくつかの特殊性があります。例えば、ABAPはメソッドを呼び出す際にデータ型を比較しているため、1つの大きなメソッドを多くのサブメソッドに分割するとコードが遅くなる可能性があります。

しかし、不明確な恐怖心から、時期尚早に最適化しないことを強くお勧めします。大多数のルール(命名、コメントなど)は全く悪影響を与えません。クリーンでオブジェクト指向的な方法で物事を構築するようにしましょう。もし、何かが遅すぎる場合は、パフォーマンスの測定を行います。その時になって初めて、選択したルールを破棄するという事実に基づいた決断をすべきです。

[Martin Fowler の _Refactoring_](https://martinfowler.com/books/refactoring.html) の第2章の一部を抜粋して、さらにいくつかの考えを述べます。

典型的なアプリケーションでは、ランタイムの大部分はコードのごく一部に費やされます。コードのわずか10%がランタイムの90%を占めることもあり、特にABAPではランタイムの大部分がデータベース時間になる可能性が高いです。

したがって、_すべての_ コードを常に超効率的にしようとすることに多大な労力を費やすのは、リソースの最善の使い方ではありません。パフォーマンスを無視しようと言っているわけではなく、開発の初期段階ではクリーンでよく構造化されたコードに注力し、それからプロファイラを使用して最適化すべき重要な領域を特定することをお勧めします。

実際、このようなアプローチは、より的を絞った最適化の努力であるため、パフォーマンスに正味のプラスの効果があり、よく構造化されたコードはパフォーマンスのボトルネックの特定や、リファクタリングやチューニングが容易になるはずです。

### 手続き型プログラミングよりもオブジェクト指向を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#手続き型プログラミングよりもオブジェクト指向を選ぶ)

オブジェクト指向のプログラム(クラスやインターフェース)は、手続き的なコード(関数やプログラム)よりもセグメント化されており、リファクタリングやテストをより容易に行うことができます。手続き的なオブジェクト (RFC のための関数、トランザクションのためのプログラム) を書かなければいけない状況もありますが、これらのオブジェクトは実際の関数を提供するクラスを呼び出すことに限定すべきです。

```ABAP
FUNCTION check_business_partner [...].
  DATA(validator) = NEW /clean/biz_partner_validator( ).
  result = validator->validate( business_partners ).
ENDFUNCTION.
```

> [Function Groups vs. Classes](sub-sections/FunctionGroupsVsClasses.md)
> に違いが詳細に書かれています。

### 手続き型の言語構造よりも関数型の言語構造を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#手続き型の言語構造よりも関数型の言語構造を選ぶ)

これらは通常、短く、モダンなプログラマーにとってはより自然なものになります。

```ABAP
DATA(variable) = 'A'.
" MOVE 'A' TO variable.

DATA(uppercase) = to_upper( lowercase ).
" TRANSLATE lowercase TO UPPER CASE.

index += 1.         " >= NW 7.54
index = index + 1.  " < NW 7.54
" ADD 1 TO index.

DATA(object) = NEW /clean/my_class( ).
" CREATE OBJECT object TYPE /dirty/my_class.

result = VALUE #( FOR row IN input ( row-text ) ).
" LOOP AT input INTO DATA(row).
"  INSERT row-text INTO TABLE result.
" ENDLOOP.

DATA(line) = value_pairs[ name = 'A' ]. " entry must exist
DATA(line) = VALUE #( value_pairs[ name = 'A' ] OPTIONAL ). " entry can be missing
" READ TABLE value_pairs INTO DATA(line) WITH KEY name = 'A'.

DATA(exists) = xsdbool( line_exists( value_pairs[ name = 'A' ] ) ).
IF line_exists( value_pairs[ name = 'A' ] ).
" READ TABLE value_pairs TRANSPORTING NO FIELDS WITH KEY name = 'A'.
" DATA(exists) = xsdbool( sy-subrc = 0 ).
```

以下に示す詳細なルールの多くは、この一般的なアドバイスの具体的な繰り返しに過ぎません。

### 廃止された言語要素を避ける

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#廃止された言語要素を避ける)

ABAPのバージョンをアップグレードする際には、廃止された言語要素を確認し、使用を控えるようにしましょう。

例えば、以下の文中の `@`-エスケープされた「host」変数は、何がプログラム変数で何がデータベースのカラムなのかを明確にしています。

```ABAP
SELECT *
  FROM spfli
  WHERE carrid = @carrid AND
        connid = @connid
  INTO TABLE @itab.
```

[廃止されたエスケープ省略形式](https://help.sap.com/doc/abapdocu_750_index_htm/7.50/en-US/abenopen_sql_hostvar_obsolete.htm) と比べて

```ABAP
SELECT *
  FROM spfli
  WHERE carrid = carrid AND
        connid = connid
  INTO TABLE itab.
```

新しい代替案はコードの可読性を向上させ、最新のプログラミング・パラダイムとの設計の衝突を減らす傾向があり、それらに切り替えることでコードを自動的にクリーンにすることができます。

廃止された要素は処理速度やメモリ消費量の面で最適化の恩恵を受けられなくなる可能性があります。

モダンな言語要素を使用することで、SAPのトレーニングではもはや教えられていないため、時代遅れの構成要素に慣れていないかもしれない若いABAPerを簡単に参加させることができます。

SAP NetWeaver のドキュメントには、廃止された言語要素がリストアップされています。例えば、
[NW 7.50](https://help.sap.com/doc/abapdocu_750_index_htm/7.50/en-US/index.htm?file=abenabap_obsolete.htm)、
[NW 7.51](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenabap_obsolete.htm)、
[NW 7.52](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/index.htm?file=abenabap_obsolete.htm)、
[NW 7.53](https://help.sap.com/doc/abapdocu_753_index_htm/7.53/en-US/index.htm?file=abenabap_obsolete.htm)。

### デザインパターンを賢く使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [言語](#言語) > [本節](#デザインパターンを賢く使う)

デザインパターンは、それが適切であり、目立った効果をもたらす場合に使用します。デザインパターンを使いたいがために、どこにでもデザインパターンを適用してはいけません。

## 定数

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#定数)

### マジックナンバーの代わりに定数を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [定数](#定数) > [本節](#マジックナンバーの代わりに定数を使う)

```ABAP
IF abap_type = cl_abap_typedescr=>typekind_date.
```

は以下より明確です

```ABAP
" アンチパターン
IF abap_type = 'D'.
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 17: Smells and Heuristics: G25:
> Replace Magic Numbers with Named Constants_ を参照してください。

### 定数インターフェースよりも列挙クラスを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [定数](#定数) > [本節](#定数インターフェースよりも列挙クラスを選ぶ)

```ABAP
CLASS /clean/message_severity DEFINITION PUBLIC ABSTRACT FINAL.
  PUBLIC SECTION.
    CONSTANTS:
      warning TYPE symsgty VALUE 'W',
      error   TYPE symsgty VALUE 'E'.
ENDCLASS.
```

または

```ABAP
CLASS /clean/message_severity DEFINITION PUBLIC CREATE PRIVATE FINAL.
  PUBLIC SECTION.
    CLASS-DATA:
      warning TYPE REF TO /clean/message_severity READ-ONLY,
      error   TYPE REF TO /clean/message_severity READ-ONLY.
  " ...
ENDCLASS.
```

関係のないものを混ぜたり、定数のコレクションが「実装されている」かもしれないと誤解させたりするのではなく

```ABAP
" アンチパターン
INTERFACE /dirty/common_constants.
  CONSTANTS:
    warning      TYPE symsgty VALUE 'W',
    transitional TYPE i       VALUE 1,
    error        TYPE symsgty VALUE 'E',
    persisted    TYPE i       VALUE 2.
ENDINTERFACE.
```

> [Enumerations](sub-sections/Enumerations.md)
> 一般的な列挙パターンについて説明し、その長所と短所を論じます。
>
> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 17: Smells and Heuristics: J3: Constants versus Enums_ を参照してください。

### 列挙クラスを使用しない場合は定数をグループ化する

> [Clean ABAP](#clean-abap) > [目次](#目次) > [定数](#定数) > [本節](#列挙クラスを使用しない場合は定数をグループ化する)

インターフェイスなどで緩く定数を集める場合は、それらをグループ化します。

```ABAP
CONSTANTS:
  BEGIN OF message_severity,
    warning TYPE symsgty VALUE 'W',
    error   TYPE symsgty VALUE 'E',
  END OF message_severity,
  BEGIN OF message_lifespan,
    transitional TYPE i VALUE 1,
    persisted    TYPE i VALUE 2,
  END OF message_lifespan.
```

次のコードよりも関係がより明確になります

```ABAP
" アンチパターン
CONSTANTS:
  warning      TYPE symsgty VALUE 'W',
  transitional TYPE i       VALUE 1,
  error        TYPE symsgty VALUE 'E',
  persisted    TYPE i       VALUE 2,
```

グループ単位のアクセスも可能です。例えば、入力のバリデーションでは

```ABAP
DO.
  ASSIGN COMPONENT sy-index OF STRUCTURE message_severity TO FIELD-SYMBOL(<constant>).
  IF sy-subrc IS INITIAL.
    IF input = <constant>.
      DATA(is_valid) = abap_true.
      RETURN.
    ENDIF.
  ELSE.
    RETURN.
  ENDIF.
ENDDO.
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 17: Smells and Heuristics: G27: Structure over Convention_ を参照してください。

## 変数

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#変数)

### 事前宣言よりもインライン宣言を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [変数](#変数) > [本節](#事前宣言よりもインライン宣言を選ぶ)

これらのガイドラインに従えば、あなたのメソッドは非常に短くなり (3-5 ステートメント)、最初に出てきた場所で変数をインライン宣言することがより自然に見えるようになります。

```ABAP
METHOD do_something.
  DATA(name) = 'something'.
  DATA(reader) = /clean/reader=>get_instance_for( name ).
  result = reader->read_it( ).
ENDMETHOD.
```

メソッドの最初に `DATA` セクションで変数を宣言するよりも

```ABAP
" アンチパターン
METHOD do_something.
  DATA:
    name   TYPE seoclsname,
    reader TYPE REF TO /dirty/reader.
  name = 'something'.
  reader = /dirty/reader=>get_instance_for( name ).
  result = reader->read_it( ).
ENDMETHOD.
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 5: Formatting: Vertical Distance: Variable Declarations_ を参照してください。

### 選択の分岐内でインライン宣言をしない

> [Clean ABAP](#clean-abap) > [目次](#目次) > [変数](#変数) > [本節](#選択の分岐内でインライン宣言をしない)

```ABAP
" アンチパターン
IF has_entries = abap_true.
  DATA(value) = 1.
ELSE.
  value = 2.
ENDIF.
```

ABAPはインライン宣言をあたかもメソッドの先頭にあるかのように扱うので、これはうまく動作します。しかし、特にメソッドが長く、すぐに宣言を見つけられない場合は、読み手を非常に混乱させてしまいます。このような場合は、インライン宣言をやめて、宣言を前に置いてください。

```ABAP
DATA value TYPE i.
IF has_entries = abap_true.
  value = 1.
ELSE.
  value = 2.
ENDIF.
```

> 詳細については [Robert C. Martin の _Clean Code_] の _Chapter 5: Formatting: Vertical Distance: Variable Declarations_ を参照してください。

### 事前宣言を連結させない

> [Clean ABAP](#clean-abap) > [目次](#目次) > [変数](#変数) > [本節](#事前宣言を連結させない)

```ABAP
DATA name TYPE seoclsname.
DATA reader TYPE REF TO reader.
```

連結は、定義された変数が論理レベルで関連していることを示唆しています。これを一貫して使用するためには、すべての連結された変数が一緒に属することを確認し、変数を追加するために追加の連結グループを導入しなければなりません。これは可能ですが、通常はその努力に見合うものではありません。

また、連結は、各行が異なるように見え、それらを変更するには、コロン、ドット、カンマをいじらなければならないので、リフォーマットやリファクタリングを不必要に複雑にします。

```ABAP
" アンチパターン
DATA:
  name   TYPE seoclsname,
  reader TYPE REF TO reader.
```

> [型句の位置を揃えない](#型句の位置を揃えない) も参照してください。  
> データ宣言の連結を使用する場合は、一緒に属する変数のグループごとに1つの連結を使用します。

### FIELD-SYMBOLよりもREF TOを選択する

> [Clean ABAP](#clean-abap) > [目次](#目次) > [変数](#変数) > [本節](#FIELD-SYMBOLよりもREF-TOを選択する)

> この節は[議論を引き起こしています](https://github.com/SAP/styleguides/issues/115)。`FIELD-SYMBOL` は、内部テーブルを反復処理する際にかなり高速になるようですが、このような場合に `REF TO` を使用することを推奨するとパフォーマンスが低下する可能性があります。

```ABAP
LOOP AT components REFERENCE INTO DATA(component).
```

の代わりに

```ABAP
" アンチパターン
LOOP AT components ASSIGNING FIELD-SYMBOL(<component>).
```

フィールドシンボルが必要な場合を除いて

```ABAP
ASSIGN generic->* TO FIELD-SYMBOL(<generic>).
ASSIGN COMPONENT name OF STRUCTURE structure TO FIELD-SYMBOL(<component>).
ASSIGN (class_name)=>(static_member) TO FIELD-SYMBOL(<member>).
```

コードレビューは、人々が「ただ〜だから」「いつもそのようにLOOPしているから」「特別な理由がないから」といった恣意的な理由で、この2つから選択する傾向があることを示しています。恣意的な選択は、なぜ一方が他方よりも使用されているのかという無用な質問に読者の時間を浪費させることになります。したがって、十分な根拠に基づく正確な決定に置き換えるべきです。私たちの推奨は、このような理由に基づいています。

- フィールドシンボルは、構造体のコンポーネントに動的にアクセスするなど、参照ではできないことができます。
  同様に、参照は、動的に型付けされたデータ構造体を構築するなど、フィールドシンボルではできないことを行うことができます。
  まとめると、1つだけに落ち着くことは不可能です。

- オブジェクト指向のABAPでは、すべてのオブジェクトが `REF TO <クラス名>` なので、参照はあらゆる場所に存在し、避けることはできません。
  対照的に、フィールドシンボルが厳密に必要とされるのは、動的な型付けに関係するごく一部の特別なケースだけです。
  そのため、オブジェクト指向のプログラムでは参照がより自然な選択となります。

- フィールドシンボルは参照よりも短いですが、結果として得られるメモリの節約は非常に小さいので、無視しても問題ありません。
  同様に、速度も問題ではありません。結果として、どちらか一方を他方よりも優先するパフォーマンス関連の理由はありません。

> 詳細はこの記事
> [_Accessing Data Objects Dynamically_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abendyn_access_data_obj_guidl.htm) を参照してください。

## テーブル

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#テーブル)

### 正しいテーブルデータ型を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#正しいテーブルデータ型を使う)

- 通常、 `HASHED` テーブルは、**1つのステップで入力され**、**変更されず**、**キーによって頻繁に読み取られる**、**大きなテーブル**に使用します。
  ハッシュテーブルには固有のメモリと処理オーバーヘッドがあるため、大量のデータと大量の読み取りアクセスに対してのみ有効です。
  テーブルの内容を変更するたびにハッシュの再計算が必要になるので、頻繁に変更されるテーブルには使用しないでください。

- 通常、`SORTED` テーブルは、**常にソートする必要があり**、**ビット単位で入力されたり**、**変更する必要があり**、**1つ以上の完全キーまたは部分キーで頻繁に読み取られたり**、**特定の順序で処理されたりする**、**大きなテーブル**に使用します。
  コンテンツを追加、変更、削除するには、適切な挿入箇所を見つける必要がありますが、テーブルのインデックスの残りの部分を調整する必要はありません。
  ソートされたテーブルは、多数の読み取りアクセスに対してのみその価値を発揮します。

- `STANDARD` テーブルは、インデックスを作成すると利点よりもオーバーヘッドが大きくなるような**小さなテーブル**や、行の順序をまったく気にしないか、追加された順に正確に処理したい **「配列」** テーブルに使用します。
また、テーブルへの異なるアクセスが必要な場合、例えば、インデックス付きアクセスや `SORT` や `BINARY SEARCH` によるソートされたアクセスがあります。

> これらはあくまでも大まかなガイドラインです。
> 詳細はこの記事 [_Selection of Table Category_ in the ABAP Language Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenitab_kind.htm) を参照してください。

### DEFAULT KEY を避ける

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#DEFAULT-KEY-を避ける)

```ABAP
" アンチパターン
DATA itab TYPE STANDARD TABLE OF row_type WITH DEFAULT KEY.
```

デフォルトキーは、新しい関数文を動作させるためだけに追加されることがよくあります。
実際のところ、キー自体は通常余計なものであり、リソースを無駄に浪費します。
数値データ型を無視するため、不明瞭な間違いにつながることさえあります。
明示的なフィールドリストがない `SORT` や `DELETE ADJACENT` 文は、内部テーブルの主キーに依存します。`DEFAULT KEY` を使用した場合、例えば数値フィールドをキーの構成要素として持つ場合、特に `READ TABLE ... BINARY` などとの組み合わせでは、非常に予期せぬ結果になる可能性があります。

キーコンポーネントを明示的に指定する

```ABAP
DATA itab2 TYPE STANDARD TABLE OF row_type WITH NON-UNIQUE KEY comp1 comp2.
```

または、キーが全く必要ない場合は `EMPTY KEY` を使ってください。

```ABAP
DATA itab1 TYPE STANDARD TABLE OF row_type WITH EMPTY KEY.
```

> [Horst Keller's blog on _Internal Tables with Empty Key_](https://blogs.sap.com/2013/06/27/abap-news-for-release-740-internal-tables-with-empty-key/) によると
>
> **注意:** `EMPTY KEY` を持つ（明示的なソートフィールドを持たない）内部テーブルでの `SORT` は全くソートされませんが、キーが空であるかどうかを静的に判断できる場合には、構文警告が出ます。

### APPEND TO よりも INSERT INTO TABLE を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#APPEND-TO-よりも-INSERT-INTO-TABLE-を選ぶ)

```ABAP
INSERT VALUE #( ... ) INTO TABLE itab.
```

`INSERT INTO TABLE` はすべてのテーブルとキーの型で動作するため、
パフォーマンス要件が変更されてもテーブルの型とキー定義のリファクタリングが容易になります。

`APPEND TO` は、`STANDARD` テーブルを配列のように使って、追加されたエントリが最終行になることを強調したい場合にのみ使用します。

### READ TABLE や LOOP AT よりも LINE_EXISTS を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#READ-TABLE-や-LOOP-AT-よりも-LINE_EXISTS-を選ぶ)

```ABAP
IF line_exists( my_table[ key = 'A' ] ).
```

次のコードよりも短く、明確に意図を表現しています。

```ABAP
" アンチパターン
READ TABLE my_table TRANSPORTING NO FIELDS WITH KEY key = 'A'.
IF sy-subrc = 0.
```

ましてや

```ABAP
" アンチパターン
LOOP AT my_table REFERENCE INTO DATA(line) WHERE key = 'A'.
  line_exists = abap_true.
  EXIT.
ENDLOOP.
```

### LOOP AT よりも READ TABLE を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#LOOP-AT-よりも-READ-TABLE-を選ぶ)

```ABAP
READ TABLE my_table REFERENCE INTO DATA(line) WITH KEY key = 'A'.
```

次のコードよりも短く、明確に意図を表現しています。

```ABAP
" アンチパターン
LOOP AT my_table REFERENCE INTO DATA(line) WHERE key = 'A'.
  EXIT.
ENDLOOP.
```

ましてや

```ABAP
" アンチパターン
LOOP AT my_table REFERENCE INTO DATA(line).
  IF line->key = 'A'.
    EXIT.
  ENDIF.
ENDLOOP.
```

### ネストした IF よりも LOOP AT WHERE を選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#ネストした-IF-よりも-LOOP-AT-WHERE-を選ぶ)

```ABAP
LOOP AT my_table REFERENCE INTO DATA(line) WHERE key = 'A'.
```

次のコードよりも短く、明確に意図を表現しています。

```ABAP
LOOP AT my_table REFERENCE INTO DATA(line).
  IF line->key = 'A'.
    EXIT.
  ENDIF.
ENDLOOP.
```

### 不要なテーブルの読み取りを避ける

> [Clean ABAP](#clean-abap) > [目次](#目次) > [テーブル](#テーブル) > [本節](#不要なテーブルの読み取りを避ける)

行があると _予想される_ 場合は、一度だけ読み取って例外に対応します。

```ABAP
TRY.
    DATA(row) = my_table[ key = input ].
  CATCH cx_sy_itab_line_not_found.
    RAISE EXCEPTION NEW /clean/my_data_not_found( ).
ENDTRY.
```

2度読み取ることで、主制御の流れを散らかして遅くするのではなく

```ABAP
" アンチパターン
IF NOT line_exists( my_table[ key = input ] ).
  RAISE EXCEPTION NEW /clean/my_data_not_found( ).
ENDIF.
DATA(row) = my_table[ key = input ].
```

> パフォーマンスの向上に加えて、これはより一般的な
> [正常系かエラー処理に集中する, 両方ではなく](#正常系かエラー処理に集中する-両方ではなく)
> の具体的なバリエーションです。

## Strings

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#strings)

### リテラルを定義するには ` を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Strings](#strings) > [本節](#リテラルを定義するには--を使う)

```ABAP
CONSTANTS some_constant TYPE string VALUE `ABC`.
DATA(some_string) = `ABC`.  " --> TYPE string
```

`'` の使用は控えましょう。余計な型変換が追加されてしまいますし、`CHAR` なのか `STRING` なのかがわからなくなります。

```ABAP
" アンチパターン
DATA some_string TYPE string.
some_string = 'ABC'.
```

`|` は一般的には問題ありませんが、`CONSTANTS` には使用できず、固定値を指定すると不要なオーバーヘッドがかかります。

```ABAP
" アンチパターン
DATA(some_string) = |ABC|.
```

### テキストを組み立てるには | を使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Strings](#strings) > [本節](#テキストを組み立てるには--を使う)

```ABAP
DATA(message) = |Received HTTP code { status_code } with message { text }|.
```

文字列テンプレートは、特にテキストに複数の変数を埋め込む場合に、何がリテラルで何が変数なのかをよりよく強調します。

```ABAP
" アンチパターン
DATA(message) = `Received an unexpected HTTP ` && status_code && ` with message ` && text.
```

## Booleans

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#booleans)

### Booleansを賢く使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Booleans](#booleans) > [本節](#Booleansを賢く使う)

一見、Booleans が自然な選択であるように見える場合はよくあります。

```ABAP
" アンチパターン
is_archived = abap_true.
```

しかし、視点を変えると、列挙がより適切な選択であったことがわかります。

```ABAP
archiving_status = /clean/archivation_status=>archiving_in_process.
```

一般的に、Booleans は、物事の種類を区別するには適しません。
なぜなら、どちらか一方だけではないケースがほぼ必ず出てくるためです。

```ABAP
assert_true( xsdbool( document->is_archived( ) = abap_true AND
                      document->is_partially_archived( ) = abap_true ) ).
```

[boolean型の入力パラメータの代わりにメソッドを分割する](#boolean型の入力パラメータの代わりにメソッドを分割する)
では、さらに、常に Boolean パラメータを疑うべき理由について説明しています。

> 詳細は
> [1](http://www.beyondcode.org/articles/booleanVariables.html)
> を参照してください。

### BooleanにはABAP_BOOLを使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Booleans](#booleans) > [本節](#BooleanにはABAP_BOOLを使う)

```ABAP
DATA has_entries TYPE abap_bool.
```

汎用型の `char1` を使用しないでください。
技術的には互換性がありますが、Boolean 変数を扱っているという事実を曖昧にしてしまいます。

また、他のBoolean型はしばしば奇妙な副作用があるので避けてください。例えば、`boolean` は3番目の値「undefined」をサポートしており、これは微妙なプログラミングエラーを引き起こします。

場合によっては、Dynpro フィールドなどのデータディクショナリエレメントが必要になることがあります。
`abap_bool` はデータディクショナリではなく型プール `abap` で定義されているため、ここでは使用できません。
この場合は、`boole_d` または `xfeld` を使用してください。
カスタム記述が必要な場合は、独自のデータエレメントを作成してください。

> ABAPは、普遍的なBooleanデータ型を持たない唯一のプログラミング言語かもしれません。
> しかし、これを持つことは欠かせません。
> この推奨はABAPプログラミングガイドラインに基づいています。

### 比較にはABAP_TRUEとABAP_FALSEを使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Booleans](#booleans) > [本節](#比較にはABAP_TRUEとABAP_FALSEを使う)

```ABAP
has_entries = abap_true.
IF has_entries = abap_false.
```

同等文字の `'X'` や `' '` や `space` は使用しないでください。
これらを使用すると、これがBoolean式であることがわかりにくくなります。

```ABAP
" アンチパターン
has_entries = 'X'.
IF has_entries = space.
```

`INITIAL` との比較は避けてください - `abap_bool` のデフォルト値が `abap_false` であることを覚えておかなければならなくなります。

```ABAP
" アンチパターン
IF has_entries IS NOT INITIAL.
```

> ABAPは、真と偽の「定数」が組み込まれていない唯一のプログラミング言語かもしれません。
> しかし、それらを持つことは欠かせません。
> この推奨はABAPプログラミングガイドラインに基づいています。

### Boolean変数をセットするにはXSDBOOLを使う

> [Clean ABAP](#clean-abap) > [目次](#目次) > [Booleans](#booleans) > [本節](#Boolean変数をセットするにはXSDBOOLを使う)

```ABAP
DATA(has_entries) = xsdbool( line IS NOT INITIAL ).
```

等価の `IF`-`THEN`-`ELSE` の方が無駄に長い。

```ABAP
" アンチパターン
IF line IS INITIAL.
  has_entries = abap_false.
ELSE.
  has_entries = abap_true.
ENDIF.
```

`xsdbool` は、boolean型 `abap_bool` に最も適した `char1` を直接生成するので、この目的には最適な方法です。
これと同等の関数 `boolc` と `boolx` は異なる型を生成し、余計な暗黙の型変換を行います。

私たちは `xsdbool` という名前が不運で誤解を招くということに同意します。
結局のところ、私たちは「xsd」という接頭辞が示唆する「XML Schema Definition」の部分には全く興味がありません。

`xsdbool` の代替案として、`COND` の三項式が考えられます。
この構文は直感的ですが、`THEN abap_true` セグメントを不必要に繰り返すため少し長くなりますし、暗黙のデフォルト値である `abap_false` の知識を必要とします。
これが二次的な解決策としてのみ提案する理由です。

```ABAP
DATA(has_entries) = COND abap_bool( WHEN line IS NOT INITIAL THEN abap_true ).
```

## 条件

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#条件)

### 条件を肯定にしてみる

> [Clean ABAP](#clean-abap) > [目次](#目次) > [条件](#条件) > [本節](#条件を肯定にしてみる)

```ABAP
IF has_entries = abap_true.
```

比較のために、同じ文を逆にするとどれだけわかりにくくなるかを見てみましょう。

```ABAP
" アンチパターン
IF has_no_entries = abap_false.
```

セクションタイトルの「してみる」は、[空のIF分岐](#空のIF分岐を作らない) のようなものが出てくるところまで無理にやってはいけないという意味です。

```ABAP
" アンチパターン
IF has_entries = abap_true.
ELSE.
  " ELSE ブロックでのみ何かを行い、IF は空のまま
ENDIF.
```

> 詳細は [Robert C. Martin の _Clean Code_] の _Chapter 17: Smells and Heuristics: G29: Avoid Negative Conditionals_ を参照してください。

### NOT ISよりもIS NOTを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [条件](#条件) > [本節](#NOT-ISよりもIS-NOTを選ぶ)

```ABAP
IF variable IS NOT INITIAL.
IF variable NP 'TODO*'.
IF variable <> 42.
```

否定は論理的には等価ですが、「頭の中で逆転」が必要になり、理解が難しくなります。

```ABAP
" アンチパターン
IF NOT variable IS INITIAL.
IF NOT variable CP 'TODO*'.
IF NOT variable = 42.
```

> これは、[条件を肯定にしてみる](#条件を肯定にしてみる) のより具体的なバリエーションです。
> ABAPプログラミングガイドラインの
> [Alternative Language Constructs](https://help.sap.com/doc/abapdocu_753_index_htm/7.53/en-US/index.htm?file=abenalternative_langu_guidl.htm)
> の節でも説明されています。

### 複素条件を分解することを考える

> [Clean ABAP](#clean-abap) > [目次](#目次) > [条件](#条件) > [本節](#複素条件を分解することを考える)

条件は、それらを構成する要素に分解すると簡単になります。

```ABAP
DATA(example_provided) = xsdbool( example_a IS NOT INITIAL OR
                                  example_b IS NOT INITIAL ).

DATA(one_example_fits) = xsdbool( applies( example_a ) = abap_true OR
                                  applies( example_b ) = abap_true OR
                                  fits( example_b ) = abap_true ).

IF example_provided = abap_true AND
   one_example_fits = abap_true.
```

すべてを一緒にするのではなく

```ABAP
" アンチパターン
IF ( example_a IS NOT INITIAL OR
     example_b IS NOT INITIAL ) AND
   ( applies( example_a ) = abap_true OR
     applies( example_b ) = abap_true OR
     fits( example_b ) = abap_true ).
```

> ABAP開発ツールのクイックフィックスを使用して、上記のように素早く条件を抽出し、変数を作成します。

### 複雑な条件を抽出することを考える

> [Clean ABAP](#clean-abap) > [目次](#目次) > [条件](#条件) > [本節](#複雑な条件を抽出することを考える)

ほとんどの場合、複雑な条件を独自のメソッドに抽出するのがよいでしょう。

```ABAP
IF is_provided( example ).

METHOD is_provided.
  DATA(is_filled) = xsdbool( example IS NOT INITIAL ).
  DATA(is_working) = xsdbool( applies( example ) = abap_true OR
                              fits( example ) = abap_true ).
  result = xsdbool( is_filled = abap_true AND
                    is_working = abap_true ).
ENDMETHOD.
```

## If

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#if)

### 空のIF分岐を作らない

> [Clean ABAP](#clean-abap) > [目次](#目次) > [If](#if) > [本節](#空のIF分岐を作らない)

```ABAP
IF has_entries = abap_false.
  " do some magic
ENDIF.
```

の方が、次のコードよりも短くてわかりやすいです。

```ABAP
" アンチパターン
IF has_entries = abap_true.
ELSE.
  " do some magic
ENDIF.
```

### 複数の択一条件にはELSE IFよりもCASEを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [If](#if) > [本節](#複数の択一条件にはELSE-IFよりもCASEを選ぶ)

```ABAP
CASE type.
  WHEN type-some_type.
    " ...
  WHEN type-some_other_type.
    " ...
  WHEN OTHERS.
    RAISE EXCEPTION NEW /clean/unknown_type_failure( ).
ENDCASE.
```

`CASE` を使用すると、相互に排他的な選択肢の組を簡単に確認できます。
一連の条件を連続して評価するのではなく、別のマイクロプロセッサ命令に変換できるため、一連の `IF` よりも高速に動作する可能性があります。
対象の変数を何度も繰り返さなくても、新しいケースを素早く追加できます。
このステートメントは、`IF`-`ELSEIF` を誤ってネストしたときに発生する可能性のあるいくつかのエラーを防ぐこともできます。

```ABAP
" アンチパターン
IF type = type-some_type.
  " ...
ELSEIF type = type-some_other_type.
  " ...
ELSE.
  RAISE EXCEPTION NEW /dirty/unknown_type_failure( ).
ENDIF.
```

### ネストの深さを浅くする

> [Clean ABAP](#clean-abap) > [目次](#目次) > [If](#if) > [本節](#ネストの深さを浅くする)

```ABAP
" アンチパターン
IF <this>.
  IF <that>.
  ENDIF.
ELSE.
  IF <other>.
  ELSE.
    IF <something>.
    ENDIF.
  ENDIF.
ENDIF.
```

ネストした `IF` はすぐに理解が難しくなり、完全なカバレッジのために指数関数的な数のテストケースが必要になります。

デシジョンツリーは通常、サブメソッドを形成し、boolean ヘルパー変数を導入することで分離することができます。

他のケースでは、以下のようにIFをマージすることで簡略化することができます。

```ABAP
IF <this> AND <that>.
```

不要なネストをするのではなく

```ABAP
" アンチパターン
IF <this>.
  IF <that>.
```

## 正規表現

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#正規表現)

### 正規表現よりもシンプルなメソッドを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [正規表現](#正規表現) > [本節](#正規表現よりもシンプルなメソッドを選ぶ)

```ABAP
IF input IS NOT INITIAL.
" IF matches( val = input  regex = '.+' ).

WHILE contains( val = input  sub = 'abc' ).
" WHILE contains( val = input  regex = 'abc' ).
```

正規表現はすぐに理解するのが難しくなります。
単純な場合は、通常、それらがない方が簡単です。

また、正規表現は通常、式ツリーに解析され、実行可能なマッチャーに実行時にコンパイルする必要があるため、より多くのメモリと処理時間を消費します。
単純な解決策としては、簡単なループと一時的変数を使用します。

### 正規表現よりも基本的なチェックを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [正規表現](#正規表現) > [本節](#正規表現よりも基本的なチェックを選ぶ)

```ABAP
CALL FUNCTION 'SEO_CLIF_CHECK_NAME'
  EXPORTING
    cls_name = class_name
  EXCEPTIONS
    ...
```

以下のように、ロジックを再発明するのではなく

```ABAP
" アンチパターン
DATA(is_valid) = matches( val     = class_name
                          pattern = '[A-Z][A-Z0-9_]{0,29}' ).
```

> 正規表現が使用される場合、Don't-Repeat-Yourself (DRY) の原則に目をつぶってしまう傾向があるようです。
> [Robert C. Martin の _Clean Code_] の _Chapter 17: Smells and Heuristics: General: G5: Duplication_ の節を参照してください。

### 複雑な正規表現は組み立てることを考える

> [Clean ABAP](#clean-abap) > [目次](#目次) > [正規表現](#正規表現) > [本節](#複雑な正規表現は組み立てることを考える)

```ABAP
CONSTANTS class_name TYPE string VALUE `CL\_.*`.
CONSTANTS interface_name TYPE string VALUE `IF\_.*`.
DATA(object_name) = |{ class_name }\|{ interface_name }|.
```

複雑な正規表現の中には、より基本的な部分からどのように構築されているかを示すと理解しやすくなるものもあります。

## クラス

> [Clean ABAP](#clean-abap) > [目次](#目次) > [本節](#クラス)

### クラス: オブジェクト指向

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [本節](#クラス-オブジェクト指向)

#### 静的クラスよりもオブジェクトを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [クラス: オブジェクト指向](#クラス-オブジェクト指向) > [本節](#静的クラスよりもオブジェクトを選ぶ)

静的クラスでは、そもそもオブジェクト指向によって得られるすべての利点が失われてしまいます。
特に、ユニットテストで本番用の依存関係をテストダブルに置き換えることはほぼ不可能です。

クラスやメソッドを静的にするかどうかを考えると、答えはほとんど常に「いいえ」になります。

このルールの例外として認められているのは、単純なユーティリティクラスです。
これらのメソッドは、特定のABAP型との相互作用を容易にします。
これらは完全にステートレスであるだけでなく、ABAPステートメントや組み込み関数のように見えるほど基本的なものです。
差別化要因は、これらのクラスはそれが使用されるコードに密結合されるので、実際にユニットテストでそれらをモックにしたくはないということです。

```ABAP
CLASS /clean/string_utils DEFINITION [...].
  CLASS-METHODS trim
   IMPORTING
     string        TYPE string
   RETURNING
     VALUE(result) TYPE string.
ENDCLASS.

METHOD retrieve.
  DATA(trimmed_name) = /clean/string_utils=>trim( name ).
  result = read( trimmed_name ).
ENDMETHOD.
```

#### 継承よりもコンポジションを選ぶ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [クラス: オブジェクト指向](#クラス-オブジェクト指向) > [本節](#継承よりもコンポジションを選ぶ)

継承を使用してクラスの階層を構築することは避けてください。そうではなく、コンポジションを優先してください。

きれいに継承を設計するのは、[リスコフの置換原則](https://en.wikipedia.org/wiki/Liskov_substitution_principle) のようなルールを尊重する必要があるため、難しいです。
また、階層の背後にある指針を理解し、消化する必要があるため、理解するのも難しいです。
継承は、メソッドがサブクラスでしか利用できなくする傾向があるため、再利用を減らします。
また、メンバーの移動や変更には、階層ツリー全体の変更が必要になる傾向があるため、リファクタリングが複雑になります。

コンポジションとは、それぞれが1つの特定の目的を果たす、小さな独立したオブジェクトを設計することを意味します。
これらのオブジェクトは、単純な委譲やファサードパターンによって、より複雑なオブジェクトに組み替えることができます。
コンポジションはより多くのクラスを生成する可能性がありますが、それ以外のデメリットはありません。

この規則があるからといって、正しい場所で継承を使うことをためらわないでください。
[コンポジットデザインパターン](https://en.wikipedia.org/wiki/Composite_pattern) など、継承が適切な用途もあります。
あなたのケースにおいて、継承が本当にデメリットよりもメリットの方が多いか、批判的に自問してみてください。
確信がもてなければ、一般的にコンポジションの方が無難です。

> [Interfaces vs. abstract classes](sub-sections/InterfacesVsAbstractClasses.md)
> では、いくつかの詳細を比較しています。

#### 同じクラスにステートフルとステートレスを混在させない

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [クラス: オブジェクト指向](#クラス-オブジェクト指向)

ステートレスとステートフルプログラミングパラダイムを同じクラスに混在させてはいけません。

ステートレスプログラミングでは、メソッドは入力を受け取り、_副作用なしに_ 出力を生成するため、メソッドがいつ、どのような順序で呼び出されても同じ結果を生成します。

```ABAP
CLASS /clean/xml_converter DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    METHODS convert
      IMPORTING
        file_content  TYPE xstring
      RETURNING
        VALUE(result) TYPE /clean/some_inbound_message.
ENDCLASS.

CLASS /clean/xml_converter IMPLEMENTATION.
  METHOD convert.
    cl_proxy_xml_transform=>xml_xstring_to_abap(
      EXPORTING
        xml       = file_content
        ext_xml   = abap_true
        svar_name = 'ROOT_NODE'
      IMPORTING
        abap_data = result ).
   ENDMETHOD.
ENDCLASS.
```

ステートフルプログラミングでは、オブジェクトの内部状態をメソッドで操作します。つまり、_副作用がいっぱいある_ ということです。

```ABAP
CLASS /clean/log DEFINITION PUBLIC CREATE PUBLIC.
  PUBLIC SECTION.
    METHODS add_message IMPORTING message TYPE /clean/message.
  PRIVATE SECTION.
    DATA messages TYPE /clean/message_table.
ENDCLASS.

CLASS /clean/log IMPLEMENTATION.
  METHOD add_message.
    INSERT message INTO TABLE messages.
  ENDMETHOD.
ENDCLASS.
```

どちらのパラダイムも問題なく、それぞれの用途があります。
しかし、それらを同じオブジェクト内に _混在させる_ と、コードが理解しにくく、不明瞭な持ち越しエラーや同期性の問題で確実に失敗するようになります。
そのようなことはしないでください。

### スコープ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [本節](#スコープ)

#### デフォルトではグローバル、必要に応じてローカルのみ

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [スコープ](#スコープ) > [本節](#デフォルトではグローバル、必要に応じてローカルのみ)

デフォルトではグローバルクラスで動作します。
適切な場所でのみローカルクラスを使用します。

> グローバルクラスは、データディクショナリに表示されるクラスです。
> ローカルクラスは、別の開発オブジェクトのインクルード内に存在し、この別のオブジェクトにのみ表示されます。

ローカルクラスは以下の場合に適しています。

- 例えば、ここでしか必要とされないグローバルクラスのデータのイテレータなど、非常に限定されたプライベートなデータ構造のため

- 例えば、特殊な目的のマルチメソッドソート集計アルゴリズムをクラスの残りのコードから分離するなど、複雑なプライベートアルゴリズムを抽出するため

- 例えば、すべてのデータベースアクセスをユニットテストのテストダブルに置き換えられる個別のローカルクラスに抽出することで、グローバルクラスの特定の側面をモックできるようにするため

ローカルクラスは他の場所では使えないので、再利用の妨げになります。
抽出するのは簡単ですが、通常、人々はそれを見つけることすらできず、望まないコードの重複につながります。
非常に長いローカルクラスのインクルードでのオリエンテーション、ナビゲーション、デバッグは面倒で煩わしいものです。
ABAPはインクルードレベルでロックするので、複数の人がローカルインクルードの異なる部分で同時に作業することができなくなります
（別々のグローバルクラスであれば可能です）。

以下のような場合は、ローカルクラスの使用を再考してください。

- ローカルインクルードが、数十ものクラスと数千行ものコードにわたっている場合
- グローバルクラスを他のクラスを保持する「パッケージ」と考えている場合
- グローバルクラスが空っぽになってしまう場合
- 別々のローカルインクルードで重複したコードが繰り返されている場合
- 開発者たちがお互いにロックアウトし始め、並行して作業できなくなる場合
- チームがお互いのローカルサブツリーを理解できなくなったため、バックログの見積もりが大幅に悪化する場合

#### 継承を意図しない場合はFINALにする

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [スコープ](#スコープ) > [本節](#継承を意図しない場合はFINALにする)

明示的に継承用に設計されていないクラスは `FINAL` にしましょう。

クラス連携を設計するとき、最初の選択は [継承ではなくコンポジション](#継承よりもコンポジションを選ぶ) であるべきです。
継承を有効にすることは、 `PROTECTED` 対 `PRIVATE` や、 [リスコフの置換原則](https://en.wikipedia.org/wiki/Liskov_substitution_principle) のようなことを考える必要があり、
多くの設計内部をフリーズさせてしまうため、軽々しく行うべきことではありません。
クラス設計でこれらのことを考慮していなかった場合は、
クラスを `FINAL` にすることで誤って継承されることを防ぐべきです。

継承にはもちろん、[コンポジット](https://en.wikipedia.org/wiki/Composite_pattern) デザインパターンなど、いくつかの適切な用途が _あります_。
ビジネスアドインもまた、サブクラスを許可して、元のコードの大部分を再利用できるようにすることで、より便利にすることができます。
ただし、これらのケースはすべて最初から設計によって継承が組み込まれていることに注意してください。

[インターフェイスを実装](#パブリックインスタンスメソッドはインタフェースの一部でなければならない) しないクリーンでないクラスは、
ユニットテストでそれらをモックすることを可能にするために、非 `FINAL` のままにしておくべきです。

#### Members PRIVATE by default, PROTECTED only if needed

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Scope](#scope) > [This section](#members-private-by-default-protected-only-if-needed)

Make attributes, methods, and other class members `PRIVATE` by default.

Make them only `PROTECTED` if you want to enable sub-classes that override them.

Internals of classes should be made available to others only on a need-to-know basis.
This includes not only outside callers but also sub-classes.
Making information over-available can cause subtle errors by unexpected redefinitions and hinder refactoring
because outsiders freeze members in place that should still be liquid.

#### Consider using immutable instead of getter

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Scope](#scope) > [This section](#consider-using-immutable-instead-of-getter)

An immutable is an object that never changes after its construction.
For this kind of object consider using public read-only attributes instead of getter methods.

```ABAP
CLASS /clean/some_data_container DEFINITION.
  PUBLIC SECTION.
    METHODS constructor
      IMPORTING
        a TYPE i
        b TYPE c
        c TYPE d.
    DATA a TYPE i READ-ONLY.
    DATA b TYPE c READ-ONLY.
    DATA c TYPE d READ-ONLY.
ENDCLASS.
```

instead of

```ABAP
CLASS /dirty/some_data_container DEFINITION.
  PUBLIC SECTION.
    METHODS get_a ...
    METHODS get_b ...
    METHODS get_c ...
  PRIVATE SECTION.
    DATA a TYPE i.
    DATA b TYPE c.
    DATA c TYPE d.
ENDCLASS.
```

> **Caution**: For objects which **do** have changing values, do not use public read-only attributes.
> Otherwise this attributes always have to be kept up to date,
> regardless if their value is needed by any other code or not.

#### Use READ-ONLY sparingly

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Scope](#scope) > [This section](#use-read-only-sparingly)

Many modern programming languages, especially Java, recommend making class members read-only
wherever appropriate to prevent accidental side effects.

While ABAP _does_ offer the `READ-ONLY` addition for data declarations, we recommend to use it sparingly.

First, the addition is only available in the `PUBLIC SECTION`, reducing its applicability drastically.
You can neither add it to protected or private members nor to local variables in a method.

Second, the addition works subtly different from what people might expect from other programming languages:
READ-ONLY data can still be modified freely from any method within the class itself, its friends, and its sub-classes.
This contradicts the more widespread write-once-modify-never behavior found in other languages.
The difference may lead to bad surprises.

> To avoid misunderstandings: Protecting variables against accidental modification is a good practice.
> We would recommend applying it to ABAP as well if there was an appropriate statement.

### Constructors

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [This section](#constructors)

#### Prefer NEW to CREATE OBJECT

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Constructors](#constructors) > [This section](#prefer-new-to-create-object)

```ABAP
DATA object TYPE REF TO /clean/some_number_range.
object = NEW #( '/CLEAN/CXTGEN' )
...
DATA(object) = NEW /clean/some_number_range( '/CLEAN/CXTGEN' ).
...
DATA(object) = CAST /clean/number_range( NEW /clean/some_number_range( '/CLEAN/CXTGEN' ) ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA object TYPE REF TO /dirty/some_number_range.
CREATE OBJECT object
  EXPORTING
    number_range = '/DIRTY/CXTGEN'.
```

except where you need dynamic types, of course

```ABAP
CREATE OBJECT number_range TYPE (dynamic_type)
  EXPORTING
    number_range = '/CLEAN/CXTGEN'.
```

#### If your global class is CREATE PRIVATE, leave the CONSTRUCTOR public

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Constructors](#constructors) > [This section](#if-your-global-class-is-create-private-leave-the-constructor-public)

```ABAP
CLASS /clean/some_api DEFINITION PUBLIC FINAL CREATE PRIVATE.
  PUBLIC SECTION.
    METHODS constructor.
```

We agree that this contradicts itself.
However, according to the article
[_Instance Constructor_ of the ABAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abeninstance_constructor_guidl.htm),
specifying the `CONSTRUCTOR` in the `PUBLIC SECTION` is required to guarantee correct compilation and syntax validation.

This applies only to global classes.
In local classes, make the constructor private, as it should be.

#### Prefer multiple static creation methods to optional parameters

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Constructors](#constructors) > [This section](#prefer-multiple-static-creation-methods-to-optional-parameters)

```ABAP
CLASS-METHODS describe_by_data IMPORTING data TYPE any [...]
CLASS-METHODS describe_by_name IMPORTING name TYPE any [...]
CLASS-METHODS describe_by_object_ref IMPORTING object_ref TYPE REF TO object [...]
CLASS-METHODS describe_by_data_ref IMPORTING data_ref TYPE REF TO data [...]
```

ABAP does not support [overloading](https://en.wikipedia.org/wiki/Function_overloading).
Use name variations and not optional parameters to achieve the desired semantics.

```ABAP
" anti-pattern
METHODS constructor
  IMPORTING
    data       TYPE any OPTIONAL
    name       TYPE any OPTIONAL
    object_ref TYPE REF TO object OPTIONAL
    data_ref   TYPE REF TO data OPTIONAL
  [...]
```

The general guideline
[_Split methods instead of adding OPTIONAL parameters_](#split-methods-instead-of-adding-optional-parameters)
explains the reasoning behind this.

Consider resolving complex constructions to a multi-step construction with the
[Builder design pattern](https://en.wikipedia.org/wiki/Builder_pattern).

#### Use descriptive names for multiple creation methods

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Constructors](#constructors) > [This section](#use-descriptive-names-for-multiple-creation-methods)

Good words to start creation methods are `new_`, `create_`, and `construct_`.
People intuitively connect them to the construction of objects.
They also add up nicely to verb phrases like `new_from_template`, `create_as_copy`, or `create_by_name`.

```ABAP
CLASS-METHODS new_describe_by_data IMPORTING p_data TYPE any [...]
CLASS-METHODS new_describe_by_name IMPORTING p_name TYPE any [...]
CLASS-METHODS new_describe_by_object_ref IMPORTING p_object_ref TYPE REF TO object [...]
CLASS-METHODS new_describe_by_data_ref IMPORTING p_data_ref TYPE REF TO data [...]
```

instead of something meaningless like

```ABAP
" anti-pattern
CLASS-METHODS create_1 IMPORTING p_data TYPE any [...]
CLASS-METHODS create_2 IMPORTING p_name TYPE any [...]
CLASS-METHODS create_3 IMPORTING p_object_ref TYPE REF TO object [...]
CLASS-METHODS create_4 IMPORTING p_data_ref TYPE REF TO data [...]
```

#### Make singletons only where multiple instances don't make sense

> [Clean ABAP](#clean-abap) > [目次](#目次) > [クラス](#クラス) > [Constructors](#constructors) > [This section](#make-singletons-only-where-multiple-instances-dont-make-sense)

```ABAP
METHOD new.
  IF singleton IS NOT BOUND.
    singleton = NEW /clean/my_class( ).
  ENDIF.
  result = singleton.
ENDMETHOD.
```

Apply the singleton pattern where your object-oriented design says
that having a second instance of something doesn't make sense.
Use it to ensure that every consumer is working with the same thing in the same state and with the same data.

Do not use the singleton pattern out of habit or because some performance rule tells you so.
It is the most overused and wrongly applied pattern and
produces unexpected cross-effects and needlessly complicates testing.
If there are no design-driven reasons for a unitary object,
leave that decision to the consumer - he can still reach the same by means outside the constructor,
for example with a factory.

## Methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#methods)

These rules apply to methods in classes and function modules.

### Calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#calls)

#### Prefer functional to procedural calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls) > [This section](#prefer-functional-to-procedural-calls)

```ABAP
modify->update( node           = /clean/my_bo_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
CALL METHOD modify->update
  EXPORTING
    node           = /dirty/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields.
```

If dynamic typing forbids functional calls, resort to the procedural style

```ABAP
CALL METHOD modify->(method_name)
  EXPORTING
    node           = /clean/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields.
```

Many of the detailed rules below are just more specific variations of this advice.

#### Omit RECEIVING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls) > [This section](#omit-receiving)

```ABAP
DATA(sum) = aggregate_values( values ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
aggregate_values(
  EXPORTING
    values = values
  RECEIVING
    result = DATA(sum) ).
```

#### Omit the optional keyword EXPORTING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls) > [This section](#omit-the-optional-keyword-exporting)

```ABAP
modify->update( node           = /clean/my_bo_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
modify->update(
  EXPORTING
    node           = /dirty/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields ).
```

#### Omit the parameter name in single parameter calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls) > [This section](#omit-the-parameter-name-in-single-parameter-calls)

```ABAP
DATA(unique_list) = remove_duplicates( list ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA(unique_list) = remove_duplicates( list = list ).
```

There are cases, however, where the method name alone is not clear enough
and repeating the parameter name may further understandability:

```ABAP
car->drive( speed = 50 ).
update( asynchronous = abap_true ).
```

#### Omit the self-reference me when calling an instance method

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls) > [This section](#omit-the-self-reference-me-when-calling-an-instance-method)

Since the self-reference `me->` is implicitly set by the system, omit it when calling an instance method

```ABAP
DATA(sum) = aggregate_values( values ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA(sum) = me->aggregate_values( values ).
```

### Methods: Object orientation

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#methods-object-orientation)

#### Prefer instance to static methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Methods: Object orientation](#methods-object-orientation) > [This section](#prefer-instance-to-static-methods)

Methods should be instance members by default.
Instance methods better reflect the "object-hood" of the class.
They can be mocked easier in unit tests.

```ABAP
METHODS publish.
```

Methods should be static only in exceptional cases, such as static creation methods.

```ABAP
CLASS-METHODS create_instance
  RETURNING
    VALUE(result) TYPE REF TO /clean/blog_post.
```

#### パブリックインスタンスメソッドはインタフェースの一部でなければならない

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Methods: Object orientation](#methods-object-orientation) > [This section](#パブリックインスタンスメソッドはインタフェースの一部でなければならない)

Public instance methods should always be part of an interface.
This decouples dependencies and simplifies mocking them in unit tests.

```ABAP
METHOD /clean/blog_post~publish.
```

In clean object orientation, having a method public without an interface does not make much sense -
with few exceptions such as enumeration classes
which will never have an alternative implementation and will never be mocked in test cases.

> [Interfaces vs. abstract classes](sub-sections/InterfacesVsAbstractClasses.md)
> describes why this also applies to classes that overwrite inherited methods.

### Parameter Number

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#parameter-number)

#### Aim for few IMPORTING parameters, at best less than three

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number) > [This section](#aim-for-few-importing-parameters-at-best-less-than-three)

```ABAP
FUNCTION seo_class_copy
  IMPORTING
    clskey      TYPE seoclskey
    new_clskey  TYPE seoclskey
    config      TYPE class_copy_config
  EXPORTING
    ...
```

would be much clearer than

```ABAP
" anti-pattern
FUNCTION seo_class_copy
  IMPORTING
    clskey                 TYPE seoclskey
    new_clskey             TYPE seoclskey
    access_permission      TYPE seox_boolean DEFAULT seox_true
    VALUE(save)            TYPE seox_boolean DEFAULT seox_true
    VALUE(suppress_corr)   TYPE seox_boolean DEFAULT seox_false
    VALUE(suppress_dialog) TYPE seox_boolean DEFAULT seox_false
    VALUE(authority_check) TYPE seox_boolean DEFAULT seox_true
    lifecycle_manager      TYPE REF TO if_adt_lifecycle_manager OPTIONAL
    lock_handle            TYPE REF TO if_adt_lock_handle OPTIONAL
    VALUE(suppress_commit) TYPE seox_boolean DEFAULT seox_false
  EXPORTING
    ...
```

Too many input parameters let the complexity of a method explode
because it needs to handle an exponential number of combinations.
Many parameters are an indicator that the method may do more than one thing.

You can reduce the number of parameters by combining them into meaningful sets with structures and objects.

#### Split methods instead of adding OPTIONAL parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number) > [This section](#split-methods-instead-of-adding-optional-parameters)

```ABAP
METHODS do_one_thing IMPORTING what_i_need TYPE string.
METHODS do_another_thing IMPORTING something_else TYPE i.
```

to achieve the desired semantic as ABAP does not support [overloading](https://en.wikipedia.org/wiki/Function_overloading).

```ABAP
" anti-pattern
METHODS do_one_or_the_other
  IMPORTING
    what_i_need    TYPE string OPTIONAL
    something_else TYPE i OPTIONAL.
```

Optional parameters confuse callers:

- Which ones are really required?
- Which combinations are valid?
- Which ones exclude each other?

Multiple methods with specific parameters for the use case avoid this confusion by giving clear guidance which parameter combinations are valid and expected.

#### Use PREFERRED PARAMETER sparingly

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number) > [This section](#use-preferred-parameter-sparingly)

The addition `PREFERRED PARAMETER` makes it hard to see which parameter is actually supplied,
making it harder to understand the code.
Minimizing the number of parameters, especially optional ones,
automatically reduces the need for `PREFERRED PARAMETER`.

#### RETURN, EXPORT, or CHANGE exactly one parameter

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number) > [This section](#return-export-or-change-exactly-one-parameter)

A good method does _one thing_, and that should be reflected by the method also returning exactly one thing.
If the output parameters of your method do _not_ form a logical entity,
your method does more than one thing and you should split it.

There are cases where the output is a logical entity that consists of multiple things.
These are easiest represented by returning a structure or object:

```ABAP
TYPES:
  BEGIN OF check_result,
    result      TYPE result_type,
    failed_keys TYPE /bobf/t_frw_key,
    messages    TYPE /bobf/t_frw_message,
  END OF check_result.

METHODS check_business_partners
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE check_result.
```

instead of

```ABAP
" anti-pattern
METHODS check_business_partners
  IMPORTING
    business_partners TYPE business_partners
  EXPORTING
    result            TYPE result_type
    failed_keys       TYPE /bobf/t_frw_key
    messages          TYPE /bobf/t_frw_message.
```

Especially in comparison to multiple EXPORTING parameters, this allows people to use the functional call style,
spares you having to think about `IS SUPPLIED` and saves people from accidentally forgetting
to retrieve a vital `ERROR_OCCURRED` information.

Instead of multiple optional output parameters, consider splitting the method according to meaningful call patterns:

```ABAP
TYPES:
  BEGIN OF check_result,
    result      TYPE result_type,
    failed_keys TYPE /bobf/t_frw_key,
    messages    TYPE /bobf/t_frw_message,
  END OF check_result.

METHODS check
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE result_type.

METHODS check_and_report
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE check_result.
```

### Parameter Types

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#parameter-types)

#### Prefer RETURNING to EXPORTING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types) > [This section](#prefer-returning-to-exporting)

```ABAP
METHODS square
  IMPORTING
    number        TYPE i
  RETURNING
    VALUE(result) TYPE i.

DATA(result) = square( 42 ).
```

Instead of the needlessly longer

```ABAP
" anti-pattern
METHODS square
  IMPORTING
    number TYPE i
  EXPORTING
    result TYPE i.

square(
  EXPORTING
    number = 42
  IMPORTING
    result = DATA(result) ).
```

`RETURNING` not only makes the call shorter,
it also allows method chaining and prevents [same-input-and-output errors](#take-care-if-input-and-output-could-be-the-same).

#### RETURNING large tables is usually okay

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types) > [This section](#returning-large-tables-is-usually-okay)

Although the ABAP language documentation and performance guides say otherwise,
we rarely encounter cases where handing over a large or deeply-nested table in a VALUE parameter
_really_ causes performance problems.
We therefore recommend to generally use

```ABAP
METHODS get_large_table
  RETURNING
    VALUE(result) TYPE /clean/some_table_type.

METHOD get_large_table.
  result = me->large_table.
ENDMETHOD.

DATA(my_table) = get_large_table( ).
```

Only if there is actual proof (= a bad performance measurement) for your individual case
should you resort to the more cumbersome procedural style

```ABAP
" anti-pattern
METHODS get_large_table
  EXPORTING
    result TYPE /dirty/some_table_type.

METHOD get_large_table.
  result = me->large_table.
ENDMETHOD.

get_large_table( IMPORTING result = DATA(my_table) ).
```

> This section contradicts the ABAP Programming Guidelines and Code Inspector checks,
> both of whom suggest that large tables should be EXPORTED by reference to avoid performance deficits.
> We consistently failed to reproduce any performance and memory deficits
> and received notice about kernel optimization that generally improves RETURNING performance.

#### Use either RETURNING or EXPORTING or CHANGING, but not a combination

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types) > [This section](#use-either-returning-or-exporting-or-changing-but-not-a-combination)

```ABAP
METHODS copy_class
  IMPORTING
    old_name      TYPE seoclsname
    new name      TYPE secolsname
  RETURNING
    VALUE(result) TYPE copy_result
  RAISING
    /clean/class_copy_failure.
```

instead of confusing mixtures like

```ABAP
" anti-pattern
METHODS copy_class
  ...
  RETURNING
    VALUE(result)      TYPE vseoclass
  EXPORTING
    error_occurred     TYPE abap_bool
  CHANGING
    correction_request TYPE trkorr
    package            TYPE devclass.
```

Different sorts of output parameters is an indicator that the method does more than one thing.
It confuses the reader and makes calling the method needlessly complicated.

An acceptable exception to this rule may be builders that consume their input while building their output:

```ABAP
METHODS build_tree
  CHANGING
    tokens        TYPE tokens
  RETURNING
    VALUE(result) TYPE REF TO tree.
```

However, even those can be made clearer by objectifying the input:

```ABAP
METHODS build_tree
  IMPORTING
    tokens        TYPE REF TO token_stack
  RETURNING
    VALUE(result) TYPE REF TO tree.
```

#### Use CHANGING sparingly, where suited

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types) > [This section](#use-changing-sparingly-where-suited)

`CHANGING` should be reserved for cases where an existing local variable
that is already filled is updated in only some places:

```ABAP
METHODS update_references
  IMPORTING
    new_reference TYPE /bobf/conf_key
  CHANGING
    bo_nodes      TYPE root_nodes.

METHOD update_references.
  LOOP AT bo_nodes REFERENCE INTO DATA(bo_node).
    bo_node->reference = new_reference.
  ENDLOOP.
ENDMETHOD.
```

Do not force your callers to introduce unnecessary local variables only to supply your `CHANGING` parameter.
Do not use `CHANGING` parameters to initially fill a previously empty variable.

#### boolean型の入力パラメータの代わりにメソッドを分割する

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types) > [This section](#boolean型の入力パラメータの代わりにメソッドを分割する)

Boolean input parameters are often an indicator
that a method does _two_ things instead of one.

```ABAP
" anti-pattern
METHODS update
  IMPORTING
    do_save TYPE abap_bool.
```

Also, method calls with a single - and thus unnamed - Boolean parameter
tend to obscure the parameter's meaning.

```ABAP
" anti-pattern
update( abap_true ).  " what does 'true' mean? synchronous? simulate? commit?
```

Splitting the method may simplify the methods' code
and describe the different intentions better

```ABAP
update_without_saving( ).
update_and_save( ).
```

Common perception suggests that setters for Boolean variables are okay:

```ABAP
METHODS set_is_deleted
  IMPORTING
    new_value TYPE abap_bool.
```

> Read more in
> [1](http://www.beyondcode.org/articles/booleanVariables.html) > [2](https://silkandspinach.net/2004/07/15/avoid-boolean-parameters/) > [3](http://jlebar.com/2011/12/16/Boolean_parameters_to_API_functions_considered_harmful..html)

### Parameter Names

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#parameter-names)

#### Consider calling the RETURNING parameter RESULT

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Names](#parameter-names) > [This section](#consider-calling-the-returning-parameter-result)

Good method names are usually so good that the `RETURNING` parameter does not need a name of its own.
The name would do little more than parrot the method name or repeat something obvious.

Repeating a member name can even produce conflicts that need to be resolved by adding a superfluous `me->`.

```ABAP
" anti-pattern
METHODS get_name
  RETURNING
    VALUE(name) TYPE string.

METHOD get_name.
  name = me->name.
ENDMETHOD.
```

In these cases, simply call the parameter `RESULT`, or something like `RV_RESULT` if you prefer Hungarian notation.

Name the `RETURNING` parameter if it is _not_ obvious what it stands for,
for example in methods that return `me` for method chaining,
or in methods that create something but don't return the created entity but only its key or so.

### Parameter Initialization

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#parameter-initialization)

#### Clear or overwrite EXPORTING reference parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Initialization](#parameter-initialization) > [This section](#clear-or-overwrite-exporting-reference-parameters)

Reference parameters refer to existing memory areas that may be filled beforehand.
Clear or overwrite them to provide reliable data:

```ABAP
METHODS square
  EXPORTING
    result TYPE i.

" clear
METHOD square.
  CLEAR result.
  " ...
ENDMETHOD.

" overwrite
METHOD square.
  result = cl_abap_math=>square( 2 ).
ENDMETHOD.
```

> Code inspector and Checkman point out `EXPORTING` variables that are never written.
> Use these static checks to avoid this otherwise rather obscure error source.

##### Take care if input and output could be the same

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Initialization](#parameter-initialization) > [This section](#take-care-if-input-and-output-could-be-the-same)

Generally, it is a good idea to clear the parameter as a first thing in the method after type and data declarations.
This makes the statement easy to spot and avoids that the still-contained value is accidentally used by later statements.

However, some parameter configurations could use the same variable as input and output.
In this case, an early `CLEAR` would delete the input value before it can be used, producing wrong results.

```ABAP
" anti-pattern
DATA value TYPE i.

square_dirty(
  EXPORTING
    number = value
  IMPORTING
    result = value ).

METHOD square_dirty.
  CLEAR result.
  result = number * number.
ENDMETHOD.
```

Consider redesigning such methods by replacing `EXPORTING` with `RETURNING`.
Also consider overwriting the `EXPORTING` parameter in a single result calculation statement.
If neither fits, resort to a late `CLEAR`.

#### Don't clear VALUE parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Initialization](#parameter-initialization) > [This section](#dont-clear-value-parameters)

Parameters that work by `VALUE` are handed over as new, separate memory areas that are empty by definition.
Don't clear them again:

```ABAP
METHODS square
  EXPORTING
    VALUE(result) TYPE i.

METHOD square.
  " no need to CLEAR result
ENDMETHOD.
```

`RETURNING` parameters are always `VALUE` parameters, so you never have to clear them:

```ABAP
METHODS square
  RETURNING
    VALUE(result) TYPE i.

METHOD square.
  " no need to CLEAR result
ENDMETHOD.
```

### Method Body

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#method-body)

#### Do one thing, do it well, do it only

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body) > [This section](#do-one-thing-do-it-well-do-it-only)

A method should do one thing, and only one thing.
It should do it in the best way possible.

A method likely does one thing if

- it has [few input parameters](#aim-for-few-importing-parameters-at-best-less-than-three)
- it [doesn't include Boolean parameters](#boolean型の入力パラメータの代わりにメソッドを分割する)
- it has [exactly one output parameter](#return-export-or-change-exactly-one-parameter)
- it is [small](#keep-methods-small)
- it [descends one level of abstraction](#descend-one-level-of-abstraction)
- it only [throws one type of exception](#throw-one-type-of-exception)
- you cannot extract meaningful other methods
- you cannot meaningfully group its statements into sections

#### 正常系かエラー処理に集中する, 両方ではなく

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body) > [This section](#正常系かエラー処理に集中す。-両方ではなく)

As a specialization of the rule [_Do one thing, do it well, do it only_](#do-one-thing-do-it-well-do-it-only),
a method should either follow the happy-path it's built for,
or the error-handling-detour in case it can't,
but probably not both.

```ABAP
" anti-pattern
METHOD append_xs.
  IF input > 0.
    DATA(remainder) = input.
    WHILE remainder > 0.
      result = result && `X`.
      remainder = remainder - 1.
    ENDWHILE.
  ELSEIF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSE.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.
```

Can be decomposed into

```ABAP
METHOD append_xs.
  validate( input ).
  DATA(remainder) = input.
  WHILE remainder > 0.
    result = result && `X`.
    remainder = remainder - 1.
  ENDWHILE.
ENDMETHOD.

METHOD validate.
  IF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSEIF input < 0.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.
```

or, to stress the validation part

```ABAP
METHOD append_xs.
  IF input > 0.
    result = append_xs_without_check( input ).
  ELSEIF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSE.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.

METHOD append_xs_without_check.
  DATA(remainder) = input.
  WHILE remainder > 0.
    result = result && `X`.
    remainder = remainder - 1.
  ENDWHILE.
ENDMETHOD.
```

#### Descend one level of abstraction

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body) > [This section](#descend-one-level-of-abstraction)

Statements in a method should be one level of abstraction below the method itself.
Correspondingly, they should all be on the same level of abstraction.

```ABAP
METHOD create_and_publish.
  post = create_post( user_input ).
  post->publish( ).
ENDMETHOD.
```

instead of confusing mixtures of low level (`trim`, `to_upper`, ...) and high level (`publish`, ...) concepts like

```ABAP
" anti-pattern
METHOD create_and_publish.
  post = NEW blog_post( ).
  DATA(user_name) = trim( to_upper( sy-uname ) ).
  post->set_author( user_name ).
  post->publish( ).
ENDMETHOD.
```

A reliable way to find out what the right level of abstraction is is this:
Let the method's author explain what the method does in few, short words, without looking at the code.
The bullets (s)he numbers are the sub-methods the method should call or the statements it should execute.

#### Keep methods small

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body) > [This section](#keep-methods-small)

Methods should have less than 20 statements, optimal around 3 to 5 statements.

```ABAP
METHOD read_and_parse_version_filters.
  DATA(active_model_version) = read_random_version_under( model_guid ).
  DATA(filter_json) = read_model_version_filters( active_model_version-guid ).
  result = parse_model_version_filters( filter_json ).
ENDMETHOD.
```

The following `DATA` declaration alone is sufficient to see that the surrounding method does way more than one thing:

```ABAP
" anti-pattern
DATA:
  class           TYPE vseoclass,
  attributes      TYPE seoo_attributes_r,
  methods         TYPE seoo_methods_r,
  events          TYPE seoo_events_r,
  types           TYPE seoo_types_r,
  aliases         TYPE seoo_aliases_r,
  implementings   TYPE seor_implementings_r,
  inheritance     TYPE vseoextend,
  friendships     TYPE seof_friendships_r,
  typepusages     TYPE seot_typepusages_r,
  clsdeferrds     TYPE seot_clsdeferrds_r,
  intdeferrds     TYPE seot_intdeferrds_r,
  attribute       TYPE vseoattrib,
  method          TYPE vseomethod,
  event           TYPE vseoevent,
  type            TYPE vseotype,
  alias           TYPE seoaliases,
  implementing    TYPE vseoimplem,
  friendship      TYPE seofriends,
  typepusage      TYPE vseotypep,
  clsdeferrd      TYPE vseocdefer,
  intdeferrd      TYPE vseoidefer,
  new_clskey_save TYPE seoclskey.
```

Of course there are occasions where it does not make sense to reduce a larger method further.
This is perfectly okay as long as the method remains [focused on one thing](#do-one-thing-do-it-well-do-it-only):

```ABAP
METHOD decide_what_to_do.
  CASE temperature.
    WHEN burning.
      result = air_conditioning.
    WHEN hot.
      result = ice_cream.
    WHEN moderate.
      result = chill.
    WHEN cold.
      result = skiing.
    WHEN freezing.
      result = hot_cocoa.
  ENDCASE.
ENDMETHOD.
```

However, it still makes sense to validate whether the verbose code hides a more suitable pattern:

```ABAP
METHOD decide_what_to_do.
  result = VALUE #( spare_time_activities[ temperature = temperature ] OPTIONAL ).
ENDMETHOD.
```

> Cutting methods very small can have bad impact on performance because it increases the number of method calls.
> The [section _Mind the performance_](#mind-the-performance) gives guidance on how to balance Clean Code and performance.

### Control flow

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [This section](#control-flow)

#### Fail fast

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow) > [This section](#fail-fast)

Validate and fail as early as possible:

```ABAP
METHOD do_something.
  IF input IS INITIAL.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
  DATA(massive_object) = build_expensive_object_from( input ).
  result = massive_object->do_some_fancy_calculation( ).
ENDMETHOD.
```

Later validations are harder to spot and understand and may have already wasted resources to get there.

```ABAP
" anti-pattern
METHOD do_something.
  DATA(massive_object) = build_expensive_object_from( input ).
  IF massive_object IS NOT BOUND. " happens if input is initial
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
  result = massive_object->do_some_fancy_calculation( ).
ENDMETHOD.
```

#### CHECK vs. RETURN

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow) > [This section](#check-vs-return)

There is no consensus on whether you should use `CHECK` or `RETURN` to exit a method
if the input doesn't meet expectations.

While `CHECK` definitely provides the shorter syntax

```ABAP
METHOD read_customizing.
  CHECK keys IS NOT INITIAL.
  " do whatever needs doing
ENDMETHOD.
```

the statement's name doesn't reveal what happens if the condition fails,
such that people will probably understand the long form better:

```ABAP
METHOD read_customizing.
  IF keys IS INITIAL.
    RETURN.
  ENDIF.
  " do whatever needs doing
ENDMETHOD.
```

You can avoid the question completely by reversing the validation
and adopting a single-return control flow

```ABAP
METHOD read_customizing.
  IF keys IS NOT INITIAL.
    " do whatever needs doing
  ENDIF.
ENDMETHOD.
```

In any case, consider whether returning nothing is really the appropriate behavior.
Methods should provide a meaningful result, meaning either a filled return parameter, or an exception.
Returning nothing is in many cases similar to returning `null`, which should be avoided.

> The [section _Exiting Procedures_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm)
> recommends using `CHECK` in this instance.
> Community discussion suggests that the statement is so unclear
> that many people will not understand the program's behavior.

#### Avoid CHECK in other positions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow) > [This section](#avoid-check-in-other-positions)

Do not use `CHECK` outside of the initialization section of a method.
The statement behaves differently in different positions and may lead to unclear, unexpected effects.

For example,
[`CHECK` in a `LOOP` ends the current iteration and proceeds with the next one](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/abapcheck_loop.htm);
people might accidentally expect it to end the method or exit the loop.
Prefer using an `IF` statement in combination with `CONTINUE` instead, since `CONTINUE` only can be used in loops.

> Based on the [section _Exiting Procedures_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm).
> Note that this contradicts the [keyword reference for `CHECK` in loops](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/abapcheck_loop.htm).

## Error Handling

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#error-handling)

### Messages

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [This section](#messages)

#### Make messages easy to find

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Messages](#messages) > [This section](#make-messages-easy-to-find)

To make messages easy to find through a where-used search from transaction SE91, use the following pattern:

```ABAP
MESSAGE e001(ad) INTO DATA(message).
```

In case variable `message` is not needed, add the pragma `##NEEDED`:

```ABAP
MESSAGE e001(ad) INTO DATA(message) ##NEEDED.
```

Avoid the following:

```ABAP
" anti-pattern
IF 1 = 2. MESSAGE e001(ad). ENDIF.
```

This is an anti-pattern since:

- It contains unreachable code.
- It tests a condition which can never be true for equality.

### Return Codes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [This section](#return-codes)

#### Prefer exceptions to return codes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Return Codes](#return-codes) > [This section](#prefer-exceptions-to-return-codes)

```ABAP
METHOD try_this_and_that.
  RAISE EXCEPTION NEW cx_failed( ).
ENDMETHOD.
```

instead of

```ABAP
" anti-pattern
METHOD try_this_and_that.
  error_occurred = abap_true.
ENDMETHOD.
```

Exceptions have multiple advantages over return codes:

- Exceptions keep your method signatures clean:
  you can return the result of the method as a `RETURNING` parameter and still throw exceptions alongside.
  Return codes pollute your signatures with additional parameters for error handling.

- The caller doesn't have to react to them immediately.
  He can simply write down the happy path of his code.
  The exception-handling `CATCH` can be at the very end of his method, or completely outside.

- Exceptions can provide details on the error in their attributes and through methods.
  Return codes require you to devise a different solution on your own, such as also returning a log.

- The environment reminds the caller with syntax errors to handle exceptions.
  Return codes can be accidentally ignored without anybody noticing.

#### Don't let failures slip through

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Return Codes](#return-codes) > [This section](#dont-let-failures-slip-through)

If you do have to use return codes, for example because you call Functions and older code not under your control,
make sure you don't let failures slip through.

```ABAP
DATA:
  current_date TYPE string,
  response     TYPE bapiret2.

CALL FUNCTION 'BAPI_GET_CURRENT_DATE'
  IMPORTING
    current_date = current_date
  CHANGING
    response     = response.

IF response-type = 'E'.
  RAISE EXCEPTION NEW /clean/some_error( );
ENDIF.
```

### Exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [This section](#exceptions)

#### Exceptions are for errors, not for regular cases

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Exceptions](#exceptions) > [This section](#exceptions-are-for-errors-not-for-regular-cases)

```ABAP
" anti-pattern
METHODS entry_exists_in_db
  IMPORTING
    key TYPE char10
  RAISING
    cx_not_found_exception.
```

If something is a regular, valid case, it should be handled with regular result parameters.

```ABAP
METHODS entry_exists_in_db
  IMPORTING
    key           TYPE char10
  RETURNING
    VALUE(result) TYPE abap_bool.
```

Exceptions should be reserved for cases that you don't expect and that reflect error situations.

```ABAP
METHODS assert_user_input_is_valid
  IMPORTING
    user_input TYPE string
  RAISING
    cx_bad_user_input.
```

Misusing exceptions misguides the reader into thinking something went wrong, when really everything is just fine.
Exceptions are also much slower than regular code because they need to be constructed
and often gather lots of context information.

#### Use class-based exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Exceptions](#exceptions) > [This section](#use-class-based-exceptions)

```ABAP
TRY.
    get_component_types( ).
  CATCH cx_has_deep_components_error.
ENDTRY.
```

The outdated non-class-based exceptions have the same features as return codes and shouldn't be used anymore.

```ABAP
" anti-pattern
get_component_types(
  EXCEPTIONS
    has_deep_components = 1
    OTHERS              = 2 ).
```

### Throwing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [This section](#throwing)

#### Use own super classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#use-own-super-classes)

```ABAP
CLASS cx_fra_static_check DEFINITION ABSTRACT INHERITING FROM cx_static_check.
CLASS cx_fra_no_check DEFINITION ABSTRACT INHERITING FROM cx_no_check.
```

Consider creating abstract super classes for each exception type for your application,
instead of sub-classing the foundation classes directly.
Allows you to `CATCH` all _your_ exceptions.
Enables you to add common functionality to all exceptions, such as special text handling.
`ABSTRACT` prevents people from accidentally using these non-descriptive errors directly.

#### Throw one type of exception

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#throw-one-type-of-exception)

```ABAP
METHODS generate
  RAISING
    cx_generation_error.
```

In the vast majority of cases, throwing multiple types of exceptions has no use.
The caller usually is neither interested nor able to distinguish the error situations.
He will therefore typically handle them all in the same way -
and if this is the case, why distinguish them in the first place?

```ABAP
" anti-pattern
METHODS generate
  RAISING
    cx_abap_generation
    cx_hdbr_access_error
    cx_model_read_error.
```

A better solution to recognize different error situations is using one exception type
but adding sub-classes that allow - but don't require - reacting to individual error situations,
as described in [Use sub-classes to enable callers to distinguish error situations](#use-sub-classes-to-enable-callers-to-distinguish-error-situations).

#### Use sub-classes to enable callers to distinguish error situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#use-sub-classes-to-enable-callers-to-distinguish-error-situations)

```ABAP
CLASS cx_bad_generation_variable DEFINITION INHERITING FROM cx_generation_error.
CLASS cx_bad_code_composer_template DEFINITION INHERITING FROM cx_generation_error.

TRY.
    generator->generate( ).
  CATCH cx_bad_generation_variable.
    log_failure( ).
  CATCH cx_bad_code_composer_template INTO DATA(bad_template_exception).
    show_error_to_user( bad_template_exception ).
  CATCH cx_generation_error INTO DATA(other_exception).
    RAISE EXCEPTION NEW cx_application_error( previous =  other_exception ).
ENDTRY.
```

If there are many different error situations, use error codes instead:

```ABAP
CLASS cx_generation_error DEFINITION ...
  PUBLIC SECTION.
    TYPES error_code_type TYPE i.
    CONSTANTS:
      BEGIN OF error_code_enum,
        bad_generation_variable    TYPE error_code_type VALUE 1,
        bad_code_composer_template TYPE error_code_type VALUE 2,
        ...
      END OF error_code_enum.
    DATA error_code TYPE error_code_type.

TRY.
    generator->generate( ).
  CATCH cx_generation_error INTO DATA(exception).
    CASE exception->error_code.
      WHEN cx_generation_error=>error_code_enum-bad_generation_variable.
      WHEN cx_generation_error=>error_code_enum-bad_code_composer_variable.
      ...
    ENDCASE.
ENDTRY.
```

#### Throw CX_STATIC_CHECK for manageable exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#throw-cx_static_check-for-manageable-exceptions)

If an exception can be expected to occur and be reasonably handled by the receiver,
throw a checked exception inheriting from `CX_STATIC_CHECK`: failing user input validation,
missing resource for which there are fallbacks, etc.

```ABAP
CLASS cx_file_not_found DEFINITION INHERITING FROM cx_static_check.

METHODS read_file
  IMPORTING
    file_name_enterd_by_user TYPE string
  RAISING
    cx_file_not_found.
```

This exception type _must_ be given in method signatures and _must_ be caught or forwarded to avoid syntax errors.
It is therefore plain to see for the consumer and ensures that (s)he won't be surprised by an unexpected exception
and will take care of reacting to the error situation.

> This is in sync with the [ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenexception_category_guidl.htm)
> but contradicts [Robert C. Martin's _Clean Code_],
> which recommends to prefer unchecked exceptions;
> [Exceptions](sub-sections/Exceptions.md) explains why.

#### Throw CX_NO_CHECK for usually unrecoverable situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#throw-cx_no_check-for-usually-unrecoverable-situations)

If an exception is so severe that the receiver is unlikely to recover from it, use `CX_NO_CHECK`:
failure to read a must-have resource, failure to resolve the requested dependency, etc.

```ABAP
CLASS cx_out_of_memory DEFINITION INHERITING FROM cx_no_check.

METHODS create_guid
  RETURNING
    VALUE(result) TYPE /bobf/conf_key.
```

`CX_NO_CHECK` _cannot_ be declared in method signatures,
such that its occurrence will come as a bad surprise to the consumer.
In the case of unrecoverable situations, this is okay
because the consumer will not be able to do anything useful about it anyway.

However, there _may_ be cases where the consumer actually wants to recognize and react to this kind of failure.
For example, a dependency manager could throw a `CX_NO_CHECK` if it's unable to provide an implementation
for a requested interface because regular application code will not be able to continue.
However, there may be a test report that tries to instantiate all kinds of things just to see if it's working,
and that will report failure simply as a red entry in a list -
this service should be able to catch and ignore the exception instead of being forced to dump.

#### Consider CX_DYNAMIC_CHECK for avoidable exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#consider-cx_dynamic_check-for-avoidable-exceptions)

Use cases for `CX_DYNAMIC_CHECK` are rare,
and in general we recommend to resort to the other exception types.
However, you may want to consider this kind of exception
as a replacement for `CX_STATIC_CHECK` if the caller has full,
conscious control over whether an exception can occur.

```ABAP
DATA value TYPE decfloat.
value = '7.13'.
cl_abap_math=>get_db_length_decs(
  EXPORTING
    in     = value
  IMPORTING
    length = DATA(length) ).
```

For example, consider the method `get_db_length_decs`
of class `cl_abap_math`, that tells you the number of digits
and decimal places of a decimal floating point number.
This method raises the dynamic exception `cx_parameter_invalid_type`
if the input parameter does not reflect a decimal floating point number.
Usually, this method will be called
for a fully and statically typed variable,
such that the developer knows
whether that exception can ever occur or not.
In this case, the dynamic exception would enable the caller
to omit the unnecessary `CATCH` clause.

#### Dump for totally unrecoverable situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#dump-for-totally-unrecoverable-situations)

If a situation is so severe that you are totally sure the receiver is unlikely to recover from it,
or that clearly indicates a programming error, dump instead of throwing an exception:
failure to acquire memory, failed index reads on a table that must be filled, etc.

```ABAP
RAISE SHORTDUMP TYPE cx_sy_create_object_error.  " >= NW 7.53
MESSAGE x666(general).                           " < NW 7.53
```

This behavior will prevent any kind of consumer from doing anything useful afterwards.
Use this only if you are sure about that.

#### Prefer RAISE EXCEPTION NEW to RAISE EXCEPTION TYPE

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing) > [This section](#prefer-raise-exception-new-to-raise-exception-type)

Note: Available from NW 7.52 onwards.

```ABAP
RAISE EXCEPTION NEW cx_generation_error( previous = exception ).
```

in general is shorter than the needlessly longer

```ABAP
RAISE EXCEPTION TYPE cx_generation_error
  EXPORTING
    previous = exception.
```

However, if you make massive use of the addition `MESSAGE`, you may want to stick with the `TYPE` variant:

```ABAP
RAISE EXCEPTION TYPE cx_generation_error
  EXPORTING
    previous = exception
  MESSAGE e136(messages).
```

### Catching

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [This section](#catching)

#### Wrap foreign exceptions instead of letting them invade your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Catching](#catching) > [This section](#wrap-foreign-exceptions-instead-of-letting-them-invade-your-code)

```ABAP
METHODS generate RAISING cx_generation_failure.

METHOD generate.
  TRY.
      generator->generate( ).
    CATCH cx_amdp_generation_failure INTO DATA(exception).
      RAISE EXCEPTION NEW cx_generation_failure( previous = exception ).
  ENDTRY.
ENDMETHOD.
```

The [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) recommends de-coupling things.
Forwarding exceptions from other components violates this principle.
Make yourself independent from the foreign code by catching those exceptions
and wrapping them in an exception type of your own.

```ABAP
" anti-pattern
METHODS generate RAISING cx_sy_gateway_failure.

METHOD generate.
  generator->generate( ).
ENDMETHOD.
```

## Comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#comments)

### Express yourself in code, not in comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#express-yourself-in-code-not-in-comments)

```ABAP
METHOD correct_day_to_last_in_month.
  WHILE is_invalid( date ).
    reduce_day_by_one( CHANGING date = date ).
  ENDWHILE.
ENDMETHOD.

METHOD is_invalid.
  DATA zero_if_invalid TYPE i.
  zero_if_invalid = date.
  result = xsdbool( zero_if_invalid = 0 ).
ENDMETHOD.

METHOD reduce_day_by_one.
  date+6(2) = date+6(2) - 1.
ENDMETHOD.
```

instead of

```ABAP
" anti-pattern
" correct e.g. 29.02. in non-leap years as well as result of a date calculation would be
" something like e.g. the 31.06. that example has to be corrected to 30.06.
METHOD fix_day_overflow.
  DO 3 TIMES.
    " 31 - 28 = 3 => this correction is required not more than 3 times
    lv_dummy = cv_date.
    " lv_dummy is 0 if the date value is a not existing date - ABAP specific implementation
    IF ( lv_dummy EQ 0 ).
      cv_date+6(2) = cv_date+6(2) - 1. " subtract 1 day from the given date
    ELSE.
      " date exists => no correction required
      EXIT.
    ENDIF.
  ENDDO.
ENDMETHOD.
```

Clean Code does _not_ forbid you to comment your code - it encourages you to exploit _better_ means,
and resort to comments only if that fails.

> This example has been challenged from a performance point of view,
> claiming that cutting the methods so small worsens performance too much.
> Sample measurements show that the refactored code is 2.13 times slower than the original dirty variant.
> The clean variant takes 9.6 microseconds to fix the input `31-02-2018`, the dirty variant only 4.5 microseconds.
> This may be a problem when the method is run very often in a high-performance application;
> for regular user input validation, it should be acceptable.
> Resort to the section [Mind the performance](#mind-the-performance) to deal with Clean Code and performance issues.

### 不適切な命名をコメントで補おうとしないでください

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#不適切な命名をコメントで補おうとしないでください)

```ABAP
DATA(input_has_entries) = has_entries( input ).
```

Improve your names instead of explaining what they really mean or why you chose bad ones.

```ABAP
" anti-pattern
" checks whether the table input contains entries
DATA(result) = check_table( input ).
```

### Use methods instead of comments to segment your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#use-methods-instead-of-comments-to-segment-your-code)

```ABAP
DATA(statement) = build_statement( ).
DATA(data) = execute_statement( statement ).
```

This not only makes the intent, structure, and dependencies of the code much clearer,
it also avoids carry-over errors when temporary variables aren't properly cleared between the sections.

```ABAP
" anti-pattern
" -----------------
" Build statement
" -----------------
DATA statement TYPE string.
statement = |SELECT * FROM d_document_roots|.

" -----------------
" Execute statement
" -----------------
DATA(result_set) = adbc->execute_sql_query( statement ).
result_set->next_package( IMPORTING data = data ).
```

### Write comments to explain the why, not the what

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#write-comments-to-explain-the-why-not-the-what)

```ABAP
" can't fail, existence of >= 1 row asserted above
DATA(first_line) = table[ 1 ].
```

Nobody needs repeating the code in natural language

```ABAP
" anti-pattern
" select alert root from database by key
SELECT * FROM d_alert_root WHERE key = key.
```

### Design goes into the design documents, not the code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#design-goes-into-the-design-documents-not-the-code)

```ABAP
" anti-pattern
" This class serves a double purpose. First, it does one thing. Then, it does another thing.
" It does so by executing a lot of code that is distributed over the local helper classes.
" To understand what's going on, let us at first ponder the nature of the universe as such.
" Have a look at this and that to get the details.
```

Nobody reads that - seriously.
If people need to read a textbook to be able to use your code,
this may be an indicator that your code has severe design issues that you should solve otherwise.
Some code _does_ need some explanation beyond a single line of comment;
consider linking the design document in these cases.

### Comment with ", not with \*

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#comment-with--not-with-)

Quote comments indent along with the statements they comment

```ABAP
METHOD do_it.
  IF input IS NOT INITIAL.
    " delegate pattern
    output = calculate_result( input ).
  ENDIF.
ENDMETHOD.
```

Asterisked comments tend to indent to weird places

```ABAP
" anti-pattern
METHOD do_it.
  IF input IS NOT INITIAL.
* delegate pattern
    output = calculate_result( input ).
  ENDIF.
ENDMETHOD.
```

### Put comments before the statement they relate to

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#put-comments-before-the-statement-they-relate-to)

```ABAP
" delegate pattern
output = calculate_result( input ).
```

Clearer than

```ABAP
" anti-pattern
output = calculate_result( input ).
" delegate pattern
```

And less invasive than

```ABAP
output = calculate_result( input ).  " delegate pattern
```

### Delete code instead of commenting it

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#delete-code-instead-of-commenting-it)

```ABAP
" anti-pattern
* output = calculate_result( input ).
```

When you find something like this, delete it.
The code is obviously not needed because your application works and all tests are green.
Deleted code can be reproduced from the version history later on.
If you need to preserve a piece of code permanently, copy it to a file or a `$TMP` or `HOME` object.

### Use FIXME, TODO, and XXX and add your ID

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#use-fixme-todo-and-xxx-and-add-your-id)

```ABAP
METHOD do_something.
  " XXX FH delete this method - it does nothing
ENDMETHOD.
```

- `FIXME` points to errors that are too small or too much in-the-making for internal incidents.
- `TODO`s are places where you want to complete something in the near(!) future.
- `XXX` marks code that works but could be better.

When you enter such a comment, add your nick, initials, or user to enable your co-developers to contact you
and ask questions if the comment is unclear.

### Don't add method signature and end-of comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#dont-add-method-signature-and-end-of-comments)

Method signature comments don't help anybody.

```ABAP
" anti-pattern
* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method CALIBRATION_KPIS=>CALCULATE_KPI
* +-------------------------------------------------------------------------------------------------+
* | [--->] STRATEGY_ID                 TYPE        STRATEGY_ID
* | [--->] THRESHOLD                   TYPE        STRATEGY_THRESHOLD
* | [--->] DETECTION_OBJECT_SCORE      TYPE        T_HIT_RESULT
* | [<---] KPI                         TYPE        T_SIMULATED_KPI
* +--------------------------------------------------------------------------------------</SIGNATURE>
```

Decades ago, when you couldn't see the method signature when inspecting its code,
or working with printouts that had dozens of pages, these comments may have made sense.
But all modern ABAP IDEs (SE24, SE80, ADT) show the method signature easily
such that these comments have become nothing but noise.

> In the form-based editor of SE24/SE80, press button _Signature_.
> In the ABAP Development Tools, mark the method name and press F2
> or add the view _ABAP Element Info_ to your perspective.

Similarly, end-of comments are superfluous.
These comments may have been helpful decades ago,
when programs and functions and the nested IFs inside were hundreds of lines of code long.
But our modern coding style produces methods short enough to readily see
what opening statement an `ENDIF` or `ENDMETHOD` belongs to:

```ABAP
" anti-pattern
METHOD get_kpi_calc.
  IF has_entries = abap_false.
    result = 42.
  ENDIF.  " IF has_entries = abap_false
ENDMETHOD.   " get_kpi_calc
```

### Don't duplicate message texts as comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#dont-duplicate-message-texts-as-comments)

```ABAP
" anti-pattern
" alert category not filled
MESSAGE e003 INTO dummy.
```

Messages change independently from your code,
and nobody will remember adjusting the comment,
such that it will outdate and even become misleading quickly
and without anybody noticing.

The modern IDEs give you easy ways to see the text behind a message,
for example in the ABAP Development Tools,
mark the message ID and press Shift+F2.

If you want it more explicit,
consider extracting the message to a method of its own.

```ABAP
METHOD create_alert_not_found_message.
  MESSAGE e003 INTO dummy.
ENDMETHOD.
```

### ABAP Doc only for public APIs

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#abap-doc-only-for-public-apis)

Write ABAP Doc to document public APIs,
meaning APIs that are intended for developers
in other teams or applications.
Don't write ABAP Doc for internal stuff.

ABAP Doc suffers from the same weaknesses as all comments,
that is, it outdates quickly and then becomes misleading.
As a consequence, you should employ it only where it makes sense,
not enforce writing ABAP Doc for each and everything.

> Read more in _Chapter 4: Good Comments: Javadocs in Public APIs_ and _Chapter 4: Bad Comments:
> Javadocs in Nonpublic Code_ of [Robert C. Martin's _Clean Code_].

### Prefer pragmas to pseudo comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments) > [This section](#prefer-pragmas-to-pseudo-comments)

Prefer pragmas to pseudo comments to suppress irrelevant warnings and errors identified by the ATC. Pseudo comments
have mostly become obsolete and have been replaced by pragmas.

```ABAP
" pattern
MESSAGE e001(ad) INTO DATA(message) ##NEEDED.

" anti-pattern
MESSAGE e001(ad) INTO DATA(message). "#EC NEEDED
```

Use program `ABAP_SLIN_PRAGMAS` or table `SLIN_DESC` to find the mapping between obsolete pseudo comments and the pragmas that
have replaced them.

## Formatting

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#formatting)

The suggestions below are [optimized for reading, not for writing](#optimize-for-reading-not-for-writing).
As ABAP's Pretty Printer doesn't cover them, some of them produce additional manual work to reformat statements
when name lengths etc. change; if you want to avoid this, consider dropping rules like
[Align assignments to the same object, but not to different ones](#align-assignments-to-the-same-object-but-not-to-different-ones).

### Be consistent

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#be-consistent)

Format all code of a project in the same way.
Let all team members use the same formatting style.

If you edit foreign code, adhere to that project's formatting style
instead of insisting on your personal style.

If you change your formatting rules over time,
use [refactoring best practices](#how-to-refactor-legacy-code)
to update your code over time.

### Optimize for reading, not for writing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#optimize-for-reading-not-for-writing)

Developers spend most time _reading_ code.
Actually _writing_ code takes up a way smaller portion of the day.

As a consequence, you should optimize your code formatting for reading and debugging, not for writing.

For example, you should prefer

```ABAP
DATA:
  a TYPE b,
  c TYPE d,
  e TYPE f.
```

to hacks such as

```ABAP
" anti-pattern
DATA:
  a TYPE b
  ,c TYPE d
  ,e TYPE f.
```

### Use the Pretty Printer before activating

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#use-the-pretty-printer-before-activating)

Apply the pretty printer - Shift+F1 in SE80, SE24, and ADT - before activating an object.

If you modify a larger unformatted legacy code base,
you may want to apply the Pretty Printer only to selected lines
to avoid huge change lists and transport dependencies.
Consider pretty-printing the complete development object
in a separate Transport Request or Note.

> Read more in _Chapter 5: Formatting: Team Rules_ of [Robert C. Martin's _Clean Code_].

### Use your Pretty Printer team settings

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#use-your-pretty-printer-team-settings)

Always use your team settings.
Specify them under
_Menu_ > _Utilities_ > _Settings ..._ > _ABAP Editor_ > _Pretty Printer_.

Set _Indent_ and _Convert Uppercase/Lowercase_ > _Uppercase Keyword_
as agreed in your team.

> [Upper vs. Lower Case](sub-sections/UpperVsLowerCase.md) explains
> why we do not give clear guidance for the type case of keywords.
>
> Read more in _Chapter 5: Formatting: Team Rules_ of [Robert C. Martin's _Clean Code_].

### No more than one statement per line

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#no-more-than-one-statement-per-line)

```ABAP
DATA do_this TYPE i.
do_this = input + 3.
```

Even if some occurrences may trick you into the misconception that this was readable:

```ABAP
" anti-pattern
DATA do_this TYPE i. do_this = input + 3.
```

### Stick to a reasonable line length

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#stick-to-a-reasonable-line-length)

Adhere to a maximum line length of 120 characters.

The human eye reads text more comfortably if the lines are not too wide -
ask a UI designer or eye movement researcher of your choice.
You will also appreciate the narrower code when debugging or comparing two sources next to each other.

The 80 or even 72 characters limit originating in the old terminal devices is a little too restrictive.
While 100 characters are often recommended and a viable choice, 120 characters seem to work a little better for ABAP,
maybe because of the general verbosity of the language.

> As a reminder you can configure in ADT the print margin to 120 characters,
> which then is visualized in the code view as a vertical line.
> Configure it under _Menu_ > _Window_ > _Preferences_ > _General_ > _Editors_ > _Text Editors_.

### Condense your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#condense-your-code)

```ABAP
DATA(result) = calculate( items ).
```

instead of adding unneeded blanks

```ABAP
" anti-pattern
DATA(result)        =      calculate(    items =   items )   .
```

### Add a single blank line to separate things, but not more

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#add-a-single-blank-line-to-separate-things-but-not-more)

```ABAP
DATA(result) = do_something( ).

DATA(else) = calculate_this( result ).
```

to highlight that the two statements do different things. But there is no reason for

```ABAP
" anti-pattern
DATA(result) = do_something( ).



DATA(else) = calculate_this( result ).
```

The urge to add separating blank lines may be an indicator that your method doesn't [do one thing](#do-one-thing-do-it-well-do-it-only).

### Don't obsess with separating blank lines

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#dont-obsess-with-separating-blank-lines)

```ABAP
METHOD do_something.
  do_this( ).
  then_that( ).
ENDMETHOD.
```

No reason for the bad habit to tear your code apart with blank lines

```ABAP
" anti-pattern
METHOD do_something.

  do_this( ).

  then_that( ).

ENDMETHOD.
```

Blank lines actually only make sense if you have statements that span multiple lines

```ABAP
METHOD do_something.

  do_this( ).

  then_that(
    EXPORTING
      variable = 'A'
    IMPORTING
      result   = result ).

ENDMETHOD.
```

### Align assignments to the same object, but not to different ones

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#align-assignments-to-the-same-object-but-not-to-different-ones)

To highlight that these things somehow belong together

```ABAP
structure-type = 'A'.
structure-id   = '4711'.
```

or even better

```ABAP
structure = VALUE #( type = 'A'
                     id   = '4711' ).
```

But leave things ragged that have nothing to do with each other:

```ABAP
customizing_reader = fra_cust_obj_model_reader=>s_get_instance( ).
hdb_access = fra_hdbr_access=>s_get_instance( ).
```

> Read more in _Chapter 5: Formatting: Horizontal Alignment_ of [Robert C. Martin's _Clean Code_].

### Close brackets at line end

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#close-brackets-at-line-end)

```ABAP
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields
).
```

### Keep single parameter calls on one line

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#keep-single-parameter-calls-on-one-line)

```ABAP
DATA(unique_list) = remove_duplicates( list ).
remove_duplicates( CHANGING list = list ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA(unique_list) = remove_duplicates(
                           list ).
DATA(unique_list) = remove_duplicates(
                         CHANGING
                           list = list ).
```

### Keep parameters behind the call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#keep-parameters-behind-the-call)

```ABAP
DATA(sum) = add_two_numbers( value_1 = 5
                             value_2 = 6 ).
```

When this makes the lines very long, you can break the parameters into the next line:

```ABAP
DATA(sum) = add_two_numbers(
                value_1 = round_up( input DIV 7 ) * 42 + round_down( 19 * step_size )
                value_2 = VALUE #( ( `Calculation failed with a very weird result` ) ) ).
```

### If you break, indent parameters under the call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#if-you-break-indent-parameters-under-the-call)

```ABAP
DATA(sum) = add_two_numbers(
                value_1 = 5
                value_2 = 6 ).
```

Aligning the parameters elsewhere makes it hard to spot what they belong to:

```ABAP
DATA(sum) = add_two_numbers(
    value_1 = 5
    value_2 = 6 ).
```

However, this is the best pattern if you want to avoid the formatting to be broken by a name length change.

### Line-break multiple parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#line-break-multiple-parameters)

```ABAP
DATA(sum) = add_two_numbers( value_1 = 5
                             value_2 = 6 ).
```

Yes, this wastes space.
However, otherwise, it's hard to spot where one parameter ends and the next starts:

```ABAP
" anti-pattern
DATA(sum) = add_two_numbers( value_1 = 5 value_2 = 6 ).
```

### Align parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#align-parameters)

```ABAP
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

Ragged margins make it hard to see where the parameter ends and its value begins:

```ABAP
" anti-pattern
modify->update( node = if_fra_alert_c=>node-item
                key = item->key
                data = item
                changed_fields = changed_fields ).
```

> This is on the other side the best pattern if you want to avoid the formatting to be broken by a name length change.

### Break the call to a new line if the line gets too long

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#break-the-call-to-a-new-line-if-the-line-gets-too-long)

```ABAP
DATA(some_super_long_param_name) =
  if_some_annoying_interface~add_two_numbers_in_a_long_name(
      value_1 = 5
      value_2 = 6 ).
```

### Indent and snap to tab

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#indent-and-snap-to-tab)

Indent parameter keywords by 2 spaces and parameters by 4 spaces:

```ABAP
DATA(sum) = add_two_numbers(
              EXPORTING
                value_1 = 5
                value_2 = 6
              CHANGING
                errors  = errors ).
```

If you have no keywords, indent the parameters by 4 spaces.

```ABAP
DATA(sum) = add_two_numbers(
                value_1 = 5
                value_2 = 6 ).
```

Use the Tab key to indent. It's okay if this adds one more space than needed.
(This happens if the `DATA(sum) =` part at the left has an uneven number of characters.)

### Indent in-line declarations like method calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#indent-in-line-declarations-like-method-calls)

Indent in-line declarations with VALUE or NEW as if they were method calls:

```ABAP
DATA(result) = merge_structures( a = VALUE #( field_1 = 'X'
                                              field_2 = 'A' )
                                 b = NEW /clean/structure_type( field_3 = 'C'
                                                                field_4 = 'D' ) ).
```

### 型句の位置を揃えない

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [This section](#型句の位置を揃えない)

```ABAP
DATA name TYPE seoclsname.
DATA reader TYPE REF TO /clean/reader.
```

A variable and its type belong together and should therefore be visually grouped in close proximity.
Aligning the `TYPE` clauses draws attention away from that and suggests that the variables form one vertical group, and their types another one.
Alignment also produces needless editing overhead, requiring you to adjust all indentations when the length of the longest variable name changes.

```ABAP
" anti-pattern
DATA name   TYPE seoclsname.
DATA reader TYPE REF TO /clean/reader.
```

## Testing

> [Clean ABAP](#clean-abap) > [Content](#content) > [This section](#testing)

### Principles

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#principles)

#### Write testable code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#write-testable-code)

Write all code in a way that allows you to test it in an automatic fashion.

If this requires refactoring your code, do it.
Do that first, before you start adding other features.

If you add to legacy code that is too badly structured to be tested,
refactor it at least to the extent that you can test your additions.

#### Enable others to mock you

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#enable-others-to-mock-you)

If you write code to be consumed by others, enable them to write unit tests for their own code,
for example by adding interfaces in all outward-facing places,
providing helpful test doubles that facilitate integration tests,
or applying dependency inversion to enable them to substitute the productive configuration with a test config.

#### Readability rules

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#readability-rules)

Make your test code even more readable than your productive code.
You can tackle bad productive code with good tests, but if you don't even get the tests, you're lost.

Keep your test code so simple and stupid that you will still understand it in a year from now.

Stick to standards and patterns, to enable your co-workers to quickly get into the code.

#### Don't make copies or write test reports

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#dont-make-copies-or-write-test-reports)

Don't start working on a backlog item by making a `$TMP` copy of a development object and playing around with it.
Others won't notice these objects and therefore won't know the status of your work.
You will probably waste a lot of time by making the working copy in the first place.
You will also forget to delete the copy afterwards, spamming your system and dependencies.
(Don't believe this? Go to your development system and check your `$TMP` right now.)

Also, don't start by writing a test report that calls something in a specific way,
and repeat that to verify that things are still working when you're working on it.
This is poor man's testing: repeating a test report by hand and verifying by eye whether everything is still fine.
Take the next step and automate this report in a unit test,
with an automatic assertion that tells you whether the code is still okay.
First, you will spare yourself the effort of having to write the unit tests afterwards.
Second, you will save a lot of time for the manual repetitions, plus avoid getting bored and tired over it.

#### Test publics, not private internals

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#test-publics-not-private-internals)

Public parts of classes, especially the interfaces they implement, are rather stable and unlikely to change.
Let your unit tests validate only the publics to make them robust
and minimize the effort you have to spend when you refactor the class.
Protected and private internals, in contrast, may change very quickly through refactoring,
such that each refactoring would needlessly break your tests.

An urgent need to test private or protected methods may be an early warning sign for several kinds of design flaws.
Ask yourself:

- Did you accidentally bury a concept in your class that wants to come out into its own class,
  with its own dedicated suite of tests?

- Did you forget to separate the domain logic from the glue code?
  For example, implementing the domain logic directly in the class that is plugged into BOPF as an action,
  determination, or validation, or that was generated by SAP Gateway as a `*_DPC_EXT` data provider, may not the best idea.

- Are your interfaces too complicated and request too much data that is irrelevant or that cannot be mocked easily?

#### Don't obsess about coverage

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles) > [This section](#dont-obsess-about-coverage)

Code coverage is there to help you find code you forgot to test, not to meet some random KPI:

Don't make up tests without or with dummy asserts just to reach the coverage.
Better leave things untested to make transparent that you cannot safely refactor them.
You can have < 100% coverage and still have perfect tests.
There are cases - such as IFs in the constructor to insert test doubles -
that may make it unpractical to reach 100%.
Good tests tend to cover the same statement multiple times, for different branches and conditions.
They will in fact have imaginary > 100% coverage.

### Test Classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#test-classes)

#### Call local test classes by their purpose

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes) > [This section](#call-local-test-classes-by-their-purpose)

Name local test classes either by the "when" part of the story

```ABAP
CLASS ltc_<public method name> DEFINITION FOR TESTING ... ."
```

or the "given" part of the story

```ABAP
CLASS ltc_<common setup semantics> DEFINITION FOR TESTING ... .
```

```ABAP
" anti-patterns
CLASS ltc_fra_online_detection_api DEFINITION FOR TESTING ... . " We know that's the class under test - why repeat it?
CLASS ltc_test DEFINITION FOR TESTING ....                      " Of course it's a test, what else should it be?
```

#### Put tests in local classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes) > [This section](#put-tests-in-local-classes)

Put unit tests into the local test include of the class under test.
This ensures that people find these tests when refactoring the class
and allows them to run all associated tests with a single key press,
as described in [How to execute test classes](#how-to-execute-test-classes).

Put component-, integration- and system tests into the local test include of a separate global class.
They do not directly relate to a single class under test, therefore they should not arbitrarily be
placed in one of the involved classes, but in a separate one.  
Mark this global test class as `FOR TESTING` and `ABSTRACT`
to avoid that it is accidentally referenced in production code.  
Putting tests into other classes has the danger that people overlook them
and forget to run them when refactoring the involved classes.

Therefore it is beneficial to use _test relations_ to document which objects
are tested by the test.  
With the example below the test class `hiring_test`
could be executed while being in the class `recruting` or `candidate` via the shrotcut `Shift-Crtl-F12` (Windows) or `Cmd-Shift-F12` (macOS).

```abap
"! @testing recruting
"! @testing candidate
class hiring_test defintion
  for testing risk level dangerous duration medium
  abstract.
  ...
endclass.
```

#### Put help methods in help classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes) > [This section](#put-help-methods-in-help-classes)

Put help methods used by several test classes in a help class. Make the help methods available through
inheritance (is-a relationship) or delegation (has-a relationship).

```abap
" inheritance example

CLASS lth_unit_tests DEFINITION ABSTRACT.

  PROTECTED SECTION.
    CLASS-METHODS assert_activity_entity
      IMPORTING
        actual_activity_entity TYPE REF TO zcl_activity_entity
        expected_activity_entity TYPE REF TO zcl_activity_entity.
    ...
ENDCLASS.

CLASS lth_unit_tests IMPLEMENTATION.

  METHOD assert_activity_entity.
    ...
  ENDMETHOD.

ENDCLASS.

CLASS ltc_unit_tests DEFINITION INHERITING FROM lth_unit_tests FINAL FOR TESTING
  DURATION SHORT
  RISK LEVEL HARMLESS.
  ...
ENDCLASS.
```

#### How to execute test classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes) > [This section](#how-to-execute-test-classes)

In the ABAP Development Tools, press Ctrl+Shift+F10 to run all tests in a class.
Press Ctrl+Shift+F11 to include coverage measurements.
Press Ctrl+Shift+F12 to also run tests in other classes that are maintained as test relations.

> On macOS, use `Cmd` instead of `Ctrl`.

### Code Under Test

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#code-under-test)

#### Name the code under test meaningfully, or default to CUT

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test) > [This section](#name-the-code-under-test-meaningfully-or-default-to-cut)

Give the variable that represents the code under test a meaningful name:

```ABAP
DATA blog_post TYPE REF TO ...
```

Don't just repeat the class name with all its non-valuable namespaces and prefixes:

```ABAP
" anti-pattern
DATA clean_fra_blog_post TYPE REF TO ...
```

If you have different test setups, it can be helpful to describe the object's varying state:

```ABAP
DATA empty_blog_post TYPE REF TO ...
DATA simple_blog_post TYPE REF TO ...
DATA very_long_blog_post TYPE REF TO ...
```

If you have problems finding a meaningful name, resort to `cut` as a default.
The abbreviation stands for "code under test".

```ABAP
DATA cut TYPE REF TO ...
```

Especially in unclean and confusing tests, calling the variable `cut`
can temporarily help the reader see what's actually tested.
However, tidying up the tests is the actual way to go for the long run.

#### Test against interfaces, not implementations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test) > [This section](#test-against-interfaces-not-implementations)

A practical consequence of the [_Test publics, not private internals_](#test-publics-not-private-internals),
type your code under test with an _interface_

```ABAP
DATA code_under_test TYPE REF TO some_interface.
```

rather than a _class_

```ABAP
" anti-pattern
DATA code_under_test TYPE REF TO some_class.
```

#### Extract the call to the code under test to its own method

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test) > [This section](#extract-the-call-to-the-code-under-test-to-its-own-method)

If the method to be tested requires a lot of parameters or prepared data,
it can help to extract the call to it to a helper method of its own that defaults the uninteresting parameters:

```ABAP
METHODS map_xml_to_itab
  IMPORTING
    xml_string TYPE string
    config     TYPE /clean/xml2itab_config DEFAULT default_config
    format     TYPE /clean/xml2itab_format DEFAULT default_format.

METHOD map_xml_to_itab.
  result = cut->map_xml_to_itab( xml_string = xml_string
                                 config     = config
                                 format     = format ).
ENDMETHOD.

DATA(itab) = map_xml_to_itab( '<xml></xml>' ).
```

Calling the original method directly can swamp your test with a lot of meaningless details:

```ABAP
" anti-pattern
DATA(itab) = cut->map_xml_to_itab( xml_string = '<xml></xml>'
                                   config     = VALUE #( 'some meaningless stuff' )
                                   format     = VALUE #( 'more meaningless stuff' ) ).
```

### Injection

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#injection)

#### Use dependency inversion to inject test doubles

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#use-dependency-inversion-to-inject-test-doubles)

Dependency inversion means that you hand over all dependencies to the constructor:

```ABAP
METHODS constructor
  IMPORTING
    customizing_reader TYPE REF TO if_fra_cust_obj_model_reader.

METHOD constructor.
  me->customizing_reader = customizing_reader.
ENDMETHOD.
```

Don't use setter injection.
It enables using the productive code in ways that are not intended:

```ABAP
" anti-pattern
METHODS set_customizing_reader
  IMPORTING
    customizing_reader TYPE REF TO if_fra_cust_obj_model_reader.

METHOD do_something.
  object->set_customizing_reader( a ).
  object->set_customizing_reader( b ). " would you expect that somebody does this?
ENDMETHOD.
```

Don't use FRIENDS injection.
It will initialize productive dependencies before they are replaced, with probably unexpected consequences.
It will break as soon as you rename the internals.
It also circumvents initializations in the constructor.

```ABAP
" anti-pattern
METHOD setup.
  cut = NEW fra_my_class( ). " <- builds a productive customizing_reader first - what will it break with that?
  cut->customizing_reader ?= cl_abap_testdouble=>create( 'if_fra_cust_obj_model_reader' ).
ENDMETHOD.

METHOD constructor.
  customizing_reader = fra_cust_obj_model_reader=>s_get_instance( ).
  customizing_reader->fill_buffer( ). " <- won't be called on your test double, so no chance to test this
ENDMETHOD.
```

#### Consider to use the tool ABAP test double

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#consider-to-use-the-tool-abap-test-double)

```ABAP
DATA(customizing_reader) = CAST /clean/customizing_reader( cl_abap_testdouble=>create( '/clean/default_custom_reader' ) ).
cl_abap_testdouble=>configure_call( customizing_reader )->returning( sub_claim_customizing ).
customizing_reader->read( 'SOME_ID' ).
```

Shorter and easier to understand than custom test doubles:

```ABAP
" anti-pattern
CLASS /dirty/default_custom_reader DEFINITION FOR TESTING CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES /dirty/customizing_reader.
    DATA customizing TYPE /dirty/customizing_table.
ENDCLASS.

CLASS /dirty/default_custom_reader IMPLEMENTATION.
  METHOD /dirty/customizing_reader~read.
    result = customizing.
  ENDMETHOD.
ENDCLASS.

METHOD test_something.
  DATA(customizing_reader) = NEW /dirty/customizing_reader( ).
  customizing_reader->customizing = sub_claim_customizing.
ENDMETHOD.
```

#### Exploit the test tools

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#exploit-the-test-tools)

In general, a clean programming style
will let you do much of the work
with standard ABAP unit tests and test doubles.
However, there are tools that will allow you
to tackle trickier cases in elegant ways:

- Use the `CL_OSQL_REPLACE` service
  to test complex OpenSQL statements
  by redirecting them to a test data bin
  that can be filled with test data
  without interfering with the rest of the system.

- Use the CDS test framework to test your CDS views.

#### Use test seams as temporary workaround

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#use-test-seams-as-temporary-workaround)

If all other techniques fail, or when in dangerous shallow waters of legacy code,
refrain to [test seams](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abaptest-seam.htm)
to make things testable.

Although they look comfortable at first sight, test seams are invasive and tend to get entangled
in private dependencies, such that they are hard to keep alive and stable in the long run.

We therefore recommend to refrain to test seams only as a temporary workaround
to allow you refactoring the code into a more testable form.

#### Use LOCAL FRIENDS to access the dependency-inverting constructor

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#use-local-friends-to-access-the-dependency-inverting-constructor)

```ABAP
CLASS /clean/unit_tests DEFINITION.
  PRIVATE SECTION.
    DATA cut TYPE REF TO /clean/interface_under_test.
    METHODS setup.
ENDCLASS.

CLASS /clean/class_under_test DEFINITION LOCAL FRIENDS unit_tests.

CLASS unit_tests IMPLEMENTATION.
  METHOD setup.
    DATA(mock) = cl_abap_testdouble=>create( '/clean/some_mock' ).
    " /clean/class_under_test is CREATE PRIVATE
     " so this only works because of the LOCAL FRIENDS
    cut = NEW /clean/class_under_test( mock ).
  ENDMETHOD.
ENDCLASS.
```

#### Don't misuse LOCAL FRIENDS to invade the tested code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#dont-misuse-local-friends-to-invade-the-tested-code)

Unit tests that access private and protected members to insert mock data are fragile:
they break when the internal structure of the tested code changes.

```ABAP
" anti-pattern
CLASS /dirty/class_under_test DEFINITION LOCAL FRIENDS unit_tests.
CLASS unit_tests IMPLEMENTATION.
  METHOD returns_right_result.
    cut->some_private_member = 'AUNIT_DUMMY'.
  ENDMETHOD.
ENDCLASS.
```

#### Don't change the productive code to make the code testable

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#dont-change-the-productive-code-to-make-the-code-testable)

```ABAP
" anti-pattern
IF me->in_test_mode = abap_true.
```

#### Don't sub-class to mock methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#dont-sub-class-to-mock-methods)

Don't sub-class and overwrite methods to mock them in your unit tests.
Although this works, it is fragile because the tests break easily when refactoring the code.
It also enables real consumers to inherit your class,
which [may hit you unprepared when not explicitly designing for it](#継承を意図しない場合はFINALにする).

```ABAP
" anti-pattern
CLASS unit_tests DEFINITION INHERITING FROM /dirty/real_class FOR TESTING [...].
  PROTECTED SECTION.
    METHODS needs_to_be_mocked REDEFINITION.
```

To get legacy code under test,
[resort to test seams instead](#use-test-seams-as-temporary-workaround).
They are just as fragile but still the cleaner way because they at least don't change the class's productive behavior,
as would happen when enabling inheritance by removing a previous `FINAL` flag or by changing method scope from `PRIVATE` to `PROTECTED`.

When writing new code, take this testability issue into account directly when designing the class,
and find a different, better way.
Common best practices include [resorting to other test tools](#exploit-the-test-tools)
and extracting the problem method to a separate class with its own interface.

> A more specific variant of
> [Don't change the productive code to make the code testable](#dont-change-the-productive-code-to-make-the-code-testable).

#### Don't mock stuff that's not needed

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#dont-mock-stuff-thats-not-needed)

```ABAP
cut = NEW /clean/class_under_test( db_reader = db_reader
                                   config    = VALUE #( )
                                   writer    = VALUE #( ) ).
```

Define your givens as precisely as possible: don't set data that your test doesn't need,
and don't mock objects that are never called.
These things distract the reader from what's really going on.

```ABAP
" anti-pattern
cut = NEW /dirty/class_under_test( db_reader = db_reader
                                   config    = config
                                   writer    = writer ).
```

There are also cases where it's not necessary to mock something at all -
this is usually the case with data structures and data containers.
For example, your unit tests may well work with the productive version of a `transient_log`
because it only stores data without any side effects.

#### Don't build test frameworks

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection) > [This section](#dont-build-test-frameworks)

Unit tests - in contrast to integration tests - should be data-in-data-out, with all test data being defined on the fly as needed.

```ABAP
cl_abap_testdouble=>configure_call( test_double )->returning( data ).
```

Don't start building frameworks that distinguish "_test case IDs_" to decide what data to provide.
The resulting code will be so long and tangled that you won't be able to keep these tests alive in the long term.

```ABAP
" anti-pattern

test_double->set_test_case( 1 ).

CASE me->test_case.
  WHEN 1.
  WHEN 2.
ENDCASE.
```

### Test Methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#test-methods)

#### Test method names: reflect what's given and expected

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods) > [This section](#test-method-names-reflect-whats-given-and-expected)

Good names reflect the given and then of the test:

```ABAP
METHOD reads_existing_entry.
METHOD throws_on_invalid_key.
METHOD detects_invalid_input.
```

Bad names reflect the when, repeat meaningless facts, or are cryptic:

```ABAP
" anti-patterns

" What's expected, success or failure?
METHOD get_conversion_exits.

" It's a test method, what else should it do but "test"?
METHOD test_loop.

" So it's parameterized, but what is its aim?
METHOD parameterized_test.

" What's "_wo_w" supposed to mean and will you still remember that in a year from now?
METHOD get_attributes_wo_w.
```

As ABAP allows only 30 characters in method names, it's fair to add an explanatory comment
if the name is too short to convey enough meaning.
ABAP Doc or the first line in the test method may be an appropriate choice for the comment.

Having lots of test methods whose names are too long may be an indicator
that you should split your single test class into several ones
and express the differences in the givens in the class's names.

#### Use given-when-then

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods) > [This section](#use-given-when-then)

Organize your test code along the given-when-then paradigm:
First, initialize stuff in a given section ("given"),
second call the actual tested thing ("when"),
third validate the outcome ("then").

If the given or then sections get so long
that you cannot visually separate the three sections anymore, extract sub-methods.
Blank lines or comments as separators may look good at first glance
but don't really reduce the visual clutter.
Still they are helpful for the reader and the novice test writer to separate the sections.

#### "When" is exactly one call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods) > [This section](#when-is-exactly-one-call)

Make sure that the "when" section of your test method contains exactly one call to the class under test:

```ABAP
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_false( is_valid ).
ENDMETHOD.
```

Calling multiple things indicates that the method has no clear focus and tests too much.
This makes it harder to find the cause when the test fails:
was it the first, second, or third call that caused the failure?
It also confuses the reader because he is not sure what the exact feature under test is.

#### Don't add a TEARDOWN unless you really need it

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods) > [This section](#dont-add-a-teardown-unless-you-really-need-it)

`teardown` methods are usually only needed to clear up database entries
or other external resources in integration tests.

Resetting members of the test class, esp. `cut` and the used test doubles, is superfluous;
they are overwritten by the `setup` method before the next test method is started.

### Test Data

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#test-data)

#### Make it easy to spot meaning

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data) > [This section](#make-it-easy-to-spot-meaning)

In unit tests, you want to be able to quickly tell which data and doubles are important,
and which ones are only there to keep the code from crashing.
Support this by giving things that have no meaning obvious names and values, for example:

```ABAP
DATA(alert_id) = '42'.                             " well-known meaningless numbers
DATA(detection_object_type) = '?=/"&'.             " 'keyboard accidents'
CONSTANTS some_random_number TYPE i VALUE 782346.  " revealing variable names
```

Don't trick people into believing something connects to real objects or real customizing if it doesn't:

```ABAP
" anti-pattern
DATA(alert_id) = '00000001223678871'.        " this alert really exists
DATA(detection_object_type) = 'FRA_SCLAIM'.  " this detection object type, too
CONSTANTS memory_limit TYPE i VALUE 4096.    " this number looks carefully chosen
```

#### Make it easy to spot differences

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data) > [This section](#make-it-easy-to-spot-differences)

```ABAP
exp_parameter_in = VALUE #( ( parameter_name = '45678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789END1' )
                            ( parameter_name = '45678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789END2' ) ).
```

Don't force readers to compare long meaningless strings to spot tiny differences.

#### Use constants to describe purpose and importance of test data

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data) > [This section](#use-constants-to-describe-purpose-and-importance-of-test-data)

```ABAP
CONSTANTS some_nonsense_key TYPE char8 VALUE 'ABCDEFGH'.

METHOD throws_on_invalid_entry.
  TRY.
      " when
      cut->read_entry( some_nonsense_key ).
      cl_abap_unit_assert=>fail( ).
    CATCH /clean/customizing_reader_error.
      " then
  ENDTRY.
ENDMETHOD.
```

### Assertions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [This section](#assertions)

#### Few, focused assertions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#few-focused-assertions)

Assert only exactly what the test method is about, and this with a small number of assertions.

```ABAP
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_false( is_valid ).
ENDMETHOD.
```

Asserting too much is an indicator that the method has no clear focus.
This couples productive and test code in too many places: changing a feature
will require rewriting a large number of tests although they are not really involved with the changed feature.
It also confuses the reader with a large variety of assertions,
obscuring the one important, distinguishing assertion among them.

```ABAP
" anti-pattern
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_false( is_valid ).
  cl_abap_unit_assert=>assert_not_initial( log->get_messages( ) ).
  cl_abap_unit_assert=>assert_equals( act = sy-langu
                                      exp = 'E' ).
ENDMETHOD.
```

#### Use the right assert type

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#use-the-right-assert-type)

```ABAP
cl_abap_unit_assert=>assert_equals( act = table
                                    exp = test_data ).
```

Asserts often do more than meets the eye, for example `assert_equals`
includes type matching and providing precise descriptions if values differ.
Using the wrong, too-common asserts will force you into the debugger immediately
instead of allowing you to see what is wrong right from the error message.

```ABAP
" anti-pattern
cl_abap_unit_assert=>assert_true( xsdbool( act = exp ) ).
```

#### Assert content, not quantity

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#assert-content-not-quantity)

```ABAP
assert_contains_exactly( actual   = table
                         expected = VALUE string_table( ( `ABC` ) ( `DEF` ) ( `GHI` ) ) ).
```

Don't write magic-number-quantity assertions if you can express the actual content you expect.
Numbers may vary although the expectations are still met.
In reverse, the numbers may match although the content is something completely unexpected.

```ABAP
" anti-pattern
assert_equals( act = lines( log_messages )
               exp = 3 ).
```

#### Assert quality, not content

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#assert-quality-not-content)

If you are interested in a meta quality of the result,
but not in the actual content itself, express that with a suitable assert:

```ABAP
assert_all_lines_shorter_than( actual_lines        = table
                               expected_max_length = 80 ).
```

Asserting the precise content obscures what you actually want to test.
It is also fragile because refactoring may produce a different
but perfectly acceptable result although it breaks all your too-precise unit tests.

```ABAP
" anti-pattern
assert_equals( act = table
               exp = VALUE string_table( ( `ABC` ) ( `DEF` ) ( `GHI` ) ) ).
```

#### Use FAIL to check for expected exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#use-fail-to-check-for-expected-exceptions)

```ABAP
METHOD throws_on_empty_input.
  TRY.
      " when
      cut->do_something( '' ).
      cl_abap_unit_assert=>fail( ).
    CATCH /clean/some_exception.
      " then
  ENDTRY.
ENDMETHOD.
```

#### Forward unexpected exceptions instead of catching and failing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#forward-unexpected-exceptions-instead-of-catching-and-failing)

```ABAP
METHODS reads_entry FOR TESTING RAISING /clean/some_exception.

METHOD reads_entry.
  "when
  DATA(entry) = cut->read_something( ).
  "then
  cl_abap_unit_assert=>assert_not_initial( entry ).
ENDMETHOD.
```

Your test code remains focused on the happy path and is therefore much easier to read and understand, as compared to:

```ABAP
" anti-pattern
METHOD reads_entry.
  TRY.
      DATA(entry) = cut->read_something( ).
    CATCH /clean/some_exception INTO DATA(unexpected_exception).
      cl_abap_unit_assert=>fail( unexpected_exception->get_text( ) ).
  ENDTRY.
  cl_abap_unit_assert=>assert_not_initial( entry ).
ENDMETHOD.
```

#### Write custom asserts to shorten code and avoid duplication

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions) > [This section](#write-custom-asserts-to-shorten-code-and-avoid-duplication)

```ABAP
METHODS assert_contains
  IMPORTING
    actual_entries TYPE STANDARD TABLE OF entries_tab
    expected_key   TYPE key_structure.

METHOD assert_contains.
  TRY.
      actual_entries[ key = expected_key ].
    CATCH cx_sy_itab_line_not_found.
      cl_abap_unit_assert=>fail( |Couldn't find the key { expected_key }| ).
  ENDTRY.
ENDMETHOD.
```

Instead of copy-pasting this over and over again.
