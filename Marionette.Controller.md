#Marionette.Controller

モジュールや、ルータのコントローラとしてコントローラや、ワークフローや他のオブジェクトやビューへの橋渡しなど、目的は多岐にわたる。


## Documentation Index

* [Basic Use](#basic-use)
* [Closing A Controller](#closing-a-controller)
* [On The Name 'Controller'](#on-the-name-controller)


#Basic Use(通常の使い方)

`Marionette.Controller`は、BackboneやMarionetterなどのオブジェクトのように、拡張可能である。`initialize`メソッドサポートし、`EventBinder`を内臓しており、自信に対してイベントを実行することが可能である。

```
//コントローラを定義する
var MyController = Marionette.Controller.extend({

  initialize: function(options){
    this.stuff = options.stuff;
  },

  doStuff: function(){
    this.trigger("stuff:done", this.stuff);
  }

});

//インスタンスを生成
var c = new MyController({
  stuff: "some stuff"
});

//イベントバインダを組み込む
c.listenTo(c, "stuff:done", function(stuff){
  console.log(stuff);
});

//doStuffを実行する
c.doStuff();

```

#Closing A Conntroller（コントローラの終了）

各コントローラインスタンスは、`close`メソッドを内臓している。`close`メソッドは、直にコントローラインスタンスにつながっている全てのイベントの操作することが可能である。コントローラから利用するEventBinder、イベントに対しても同様に操作出来る。

`close`メソッドは、"close"イベントを発火させ、`onClose`メソッドに対応している。

```
//onCloseメソッドともにコントローラを定義する
var MyController = Marionette.Controller.extend({

  onClose: function(){
    // コントローラを終了させるためのコードを各
  }
On The Name 'Controller'
})

// 新しいコントローラのインスタンスを生成する
var contr = new MyController();

// イベントハンドラーを追加する
contr.on("close", function(){ ... });
contr.listenTo(something, "bar", function(){...});

// close the controller: unbind all of the event handlers, trigger the "close" event and call the onClose method
// コントローラを終了させる: 全てのイベントハンドラーをアンバインドして、'close'イベントを
//発火させる、onCloseメソッドを呼ぶ.
controller.close();
```

#On The Name 'Controller'(コントローラという名前について)

`Controller`という名前は、若干の混乱を招いていて、結構不幸なことである。このオブジェクトをなんと呼ぶべきか、MVCスタイルのコントローラにれ親しんだ人々が混乱するのではないか、と議論を呼んでいる。最終的に、作者はとにかくこれをコントローラと呼ぶことを決めた。スタックトレースやアプリケーションのプロセスと（または）モジュールをコントロースするために、一般的に、である。

しかし、実際これはとても一般的で、多くの目的を持つオブジェクトは、様々なシナリオの中で様々な役割を果たさなければいけない。我々は、常により記述性に溢れ、混乱を招かないための命名に対する提言を待っている。もし、違う名前を提案しければ、メーリスやissueに書いてくれ。






