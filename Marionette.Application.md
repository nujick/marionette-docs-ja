

##イニシャライザを追加する(Adding Initializer)

アプリケーションを開発するときには、リージョン内のコンテントを表示したり、ルータを設定したり、する機能を簡単に表記出来なければいけない。これらの目標を達成するため、アプリケーションが真にカスタマイズ出来ることや、アプリにコールバックを追加できることを証明しなければいけない。

```
MyApp.addInitializer(function(options){
  // do useful stuff here
  var myView = new MyView({
    model: options.someModel
  });
  MyApp.mainRegion.show(myView);
});

MyApp.addInitializer(function(options){
  new MyAppRouter();
  Backbone.history.start();
});
```

これらのコールバックは、アプリケーションのスタート時に実行されて、アプリケーションのオブジェクトに対して、コールバックに対する一連の動作としてバインドされる。つまり、**MyApp**オブジェクトは、イニシャライザの中身である。

**option**パラメータは**startメソッド**を通ってくる（下記参照）

イニシャライザコールバックは、どんなアプリケーションオブジェクトを追加したとしても実行されることが保障されている。仮に、アプリケーションがスタートする前に、スタートメソッドが呼ばれた時にイニシャライザコールバックは呼ばれる。それらをアプリケーションがスタートした後に加えられたなら、イニシャライザは即座に実行される。


##アプリケーションイベント(Application Event)

**Application**オブジェクトは、ライフサイクル間に**Marionette.triggerMethod**関数を用いたイベントを殆ど引き起こすことはない。これらのイベントは、更なるアプリケーションのプロセスを上乗せするときに利用されることがある。例えば、アプリを開発するときに、初期化が怒る前に何かデータを処理したいとする。また、**Backbone.history**を実行するためにアプリを初期化処理を待機させたいとする。

イベントのトリガーは、

- **initialize:before / ** ```onInitializeBefore```: イニシャライザの処理が始まる前に発火
- **initialize:after/** ```onInitiazlizeAfter```: 初期化処理のちょうど終了時に発火
- **start/** ```onstart```:イニシャライザとイニシャライザのイベントの終了後に発火

```
MyApp.on("initialize:before", function(options){
  options.moreData = "Yo dawg, I heard you like options so I put some options in your options!"
});

MyApp.on("initialize:after", function(options){
  if (Backbone.history){
    Backbone.history.start();
  }
});
```

```option```パラメータは、アプリケーションオブジェクトの```start```メソッド(下記参照)を通って呼ばれる。



##アプリをスタートする（Starting Application）

一度、アプリケーションのカスタマイズすると、```MyApp.start(options)```を呼ぶことで全てのイベントを呼ぶことが出来る。

```MyApp.start(options)```は、ひとつのオプションパラメータを取るり、初期化イベントだけでなく、初期化関数を通ってくいく。このことにより、アプリのスタート時や、単なる定義だけでなく、更なるカスタマイズをアプリケーションに対して加える事が出来る。


```
var options = {
  something: "some value",
  another: "#some-selector"
};

MyApp.start(options);
```


##イベントアグリゲータ app.vent: Event Aggregator

全てのアプリケーションインスタンスは、```Backbone.Wreqr.EventAggregator```が```app.vent```を呼びながら、現れる。


```
MyApp = new Backbone.Marionette.Application();

MyApp.vent.on("foo", function(){
  alert("bar");
});

MyApp.vent.trigger("foo"); // => alert box "bar"
```

詳細は、```Backbone.Wreqr```を読むこと


##Regions And The Application Object

Marionette`sの```Region```オブジェクトは、直にアプリケーションの```addRegions```メソッドを呼ぶことに寄って、追加することが出来る。
これらはアプリケーションに対してリージョンを追加する3つの方法がある。

###jQuery Selector

始めは、jQueryセレクタをリージョン定義のvalueとして定義することだ。これは、Marionette.Regionのインスンスをダイレクトに生成して、セレクタに対してアサインする

```
MyApp.addRegions({
  someRegion: "#some-div",
  anotherRegion: "#another-div"
});
```


###Custom Region Type

２つ目は、カスタムRegionタイプを、定義する。定義場所は、Regionタイプが定義されている所だ。

```
MyCustomRegion = Marionette.Region.extend({
  el: "#foo"
});

MyApp.addRegions({
  someRegion: MyCustomRegion
});

```


###Custom Region Type And Selector

３つ目のやりかたは、カスタムRegionタイプとjQueryセレクタをregionインスタンに対して、オブジェクトリテラルを用いて定義することだ。

```
MyCustomRegion = Marionette.Region.extend({});

MyApp.addRegions({

  someRegion: {
    selector: "#foo",
    regionType: MyCustomRegion
  },

  anotherRegion: {
    selector: "#bar",
    regionType: MyCustomRegion
  }

});
```

###Get Region By Name

regionは、```getRegion```メソッドを用いてnameによりgetterとして振る舞う。

```
var app = new Marionette.Application();
app.addRegions({ r1: "#region1" });

// r1 === r1Again; true
var r1 = app.getRegion("r1");
var r1Again = app.r1;
```

###Removing Regions

RegionsはRegionの名前をvalueとして取り、```removeRegion```メソッドによって削除されうる。

```
MyApp.removeRegion('someRegion');
```

Regionを削除することが、正しく、アプリケーションオブジェクトから削除する前に閉じることが出来る。(?)
詳細はRegionのdocsで。
