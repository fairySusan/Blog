MVC是数个设计模式结合起来的模式。

我们将用MVC模式来实现一个节拍控制器，节拍控制器有控制节拍设置为每分钟多少拍（Beat Per Minutes）的控制中心（视图）和当前是多少节拍的显示（视图）

认识模型、视图、控制器

模型：是实现控制节拍器开始、停止、设置节拍、获取节拍的逻辑部分。

模型：是负责所有数据、状态和应用逻辑，当‘模型’发生改变，需要去通知视图更新，所以‘模型’使用观察者模式。
```ts
interface  BeatModelInterface {
  initislize (): void;
  on (): void;
  off (): void;
  setBPM(bpm: number): void;
  getBPM(): number;
  registerBeatObserver(o: BeatObserver); //打拍子的时候通知观察者
  removeBeatObserver(o: BeatObserver);
  registerBPMObserver(o: BPMObserver); // 当设置BPM的时候通知观察者
  removeBPMObserver(o: BPMObserver) 
}
```

现在我们来实现一个具体的BeatModel类
```ts
class BeatModel implements BeatModelInterface, MetaEventListener {
  sequencer: Sequencer; // 它去产生真实的拍子（这里指人能听到的拍子）

  beatObservers = [];
  bpmObservers = [];

  bpm = 90 // 默认每分钟90次拍子

  initislize () {
    // 用于设置定序器和节拍音轨 （不用管）
    this.setUpMidi();
    this.buildTrackAndStart();
  }

  on () {
    this.sequencer.start();
    this.setBPM(90)
  }

  off () {
    this.setBPM(0);
    this.sequencer.stop();
  }

  setBPM (bpm: number) {
    this.bpm = bpm;
    this.sequencer.setTempoInBPM(this.getBPM()) // 设置新的BPM
    this.notifyBPMObservers(); // 通知观察者BPM有已经改变
  }

  getBPM () {
    return this.bpm
  }

  beatEvent () {
    notifyBeatObserver();
  }
  // 未完待续...

}
```

下面我们要将视图挂接上，使BeatModel可视化。视图分为模型的视图和用户界面控制的视图

```ts
class DJView implements ActionListener, BeatObserver, BPMPObserver {
  model: BeatModelInterface;
  controller: ControllerInterface;
  viewFrame: JFrame;
  viewPanel: JPanel;
  beatBar: BeatBar;
  bpmOutputLabel: JLabel;

  DJView(controller: ControllerInterface, model: BeatModelInterface) {
    this.controller = controller;
    this.model = model;
    model.registerBeatObserver(this); 
    model.registerBMPObserver(this);
  }

  createView () {

  }

  updateBPM () {
    let bpm = model.getBPM();
    if (bpm === 0) {
      bpmOutputLabel.setText('offline')
    } else {
      bpmOutputLabel.setText('Current BPM: ' + model.getBPM());
    }
  }

  updateBeat () {
    beatBar.setValue(100)
  }
}
```