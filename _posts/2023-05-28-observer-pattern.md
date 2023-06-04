---
layout: post
title: "Observer Pattern"
description: ""
date: 2023-05-28
tags: [pattern]
---

<a href="http://www.yes24.com/Product/Goods/12501269">자바 객체지향 디자인 패턴</a>

![0](/assets/images/observer-pattern/0.png)

```java
public class ScoreRecord {
  private List<Integer> scores = new ArrayList<>();
  private DataSheetView dataSheetView;

  public void setDataSheetView(DataSheetView dataSheetView) {
    this.dataSheetView = dataSheetView;
  }

  public void addScore(int score) {
    scores.add(score);
    dataSheetView.update();
  }

  public List<Integer> getScoreRecord() {
    return scores;
  }
}

public class DataSheetView {
  private ScoreRecord scoreRecord;
  private int viewCount;

  public DataSheetView(ScoreRecord scoreRecord, int viewCount) {
    this.scoreRecord = scoreRecord;
    this.viewCount = viewCount;
  }

  public void update() {
    List<Integer> record = scoreRecord.getScoreRecord();
    displayScores(record, viewCount);
  }

  private void displayScores(List<Integer> record, int viewCount) {
    System.out.println("List of " + viewCount + " entries: ");
    for (int i = 0; i < viewCount && i < record.size(); i++) {
      System.out.print(record.get(i) + " ");
    }
    System.out.println();
  }
}

public class Client {

  public static void main(String[] args) {
    ScoreRecord scoreRecord = new ScoreRecord();
    DataSheetView dataSheetView = new DataSheetView(scoreRecord, 3);
    scoreRecord.setDataSheetView(dataSheetView);

    for (int index = 1; index <= 5; index++) {
      int score = index * 10;
      System.out.println("Adding " + score);
      scoreRecord.addScore(score);
    }
  }
}
```

ScoreRecord 클래스는 점수를 관리, 저장하며 DataSheetView 클래스는 점수를 출력하는 기능을 가지고 있다.

ScoreRecord 객체에 점수를 추가한 후 최신 데이터를 반영하기 위해 DataSheetView 클래스의 update 메서드를 호출한다. (변경한 점수를 View 클래스에 업데이트)

DataSheetView 클래스는 변경된 최신 점수를 출력한다.

**하지만 다른 형태로 성적을 출력하고 싶다면 어떻게 해야할까?**

ScoreRecord 클래스에 선언된 View 클래스를 다른 형태의 View 클래스로 수정해야 하기 때문에 OCP를 위배한다.

즉 다른 형태의 클래스를 추가하더라도 ScoreRecord 클래스를 수정하지 않고 그대로 사용할 수 있어야 한다.

![1](/assets/images/observer-pattern/1.png)

```java
public interface Observer {
  public abstract void update();
}

public class DataSheetView implements Observer {
  private ScoreRecord scoreRecord;
  private int viewCount;

  public DataSheetView(ScoreRecord scoreRecord, int viewCount) {
    this.scoreRecord = scoreRecord;
    this.viewCount = viewCount;
  }

  @Override
  public void update() {
    List<Integer> record = scoreRecord.getScoreRecord();
    displayScores(record, viewCount);
  }

  private void displayScores(List<Integer> record, int viewCount) {
    System.out.println("List of " + viewCount + " entries: ");
    for (int i = 0; i < viewCount && i < record.size(); i++) {
      System.out.print(record.get(i) + " ");
    }
    System.out.println();
  }
}

public class MinMaxView implements Observer {
  private ScoreRecord scoreRecord;

  public MinMaxView(ScoreRecord scoreRecord) {
    this.scoreRecord = scoreRecord;
  }

  @Override
  public void update() {
    List<Integer> record = scoreRecord.getScoreRecord();
    displayMinMax(record);
  }

  private void displayMinMax(List<Integer> record) {
    int min = Collections.min(record, null);
    int max = Collections.max(record, null);
    System.out.println("Min: " + min + " Max: " + max);
  }
}

public class StatisticsView implements Observer {
  private ScoreRecord scoreRecord;

  public StatisticsView(ScoreRecord scoreRecord) {
    this.scoreRecord = scoreRecord;
  }

  @Override
  public void update() {
    List<Integer> record = scoreRecord.getScoreRecord();
    displayStatistics(record);
  }

  public void displayStatistics(List<Integer> record) {
    int sum = 0;
    for (int score: record)
      sum += score;

    float average = (float) sum / record.size();
    System.out.println("Sum: " + sum + " Average: " + average);
  }
}

public abstract class Subject {
  private List<Observer> observers = new ArrayList<>();

  public void attach(Observer observer) {
    observers.add(observer);
  }

  public void detach(Observer observer) {
    observers.remove(observer);
  }

  public void notifyObservers() {
    for (Observer o: observers)
      o.update();
  }
}

public class ScoreRecord extends Subject {
  private List<Integer> scores = new ArrayList<>();

  public void addScore(int score) {
    scores.add(score);
    notifyObservers();
  }

  public List<Integer> getScoreRecord() {
    return scores;
  }
}

public class Client {
  public static void addScores(ScoreRecord scoreRecord) {
    for (int index = 1; index <= 5; index++) {
      int score = index * 10;
      System.out.println("Adding " + score);
      scoreRecord.addScore(score);
    }
  }

  public static void main(String[] args) {
    ScoreRecord scoreRecord = new ScoreRecord();
    DataSheetView dataSheetView3 = new DataSheetView(scoreRecord, 3);
    DataSheetView dataSheetView5 = new DataSheetView(scoreRecord, 5);
    MinMaxView minMaxView = new MinMaxView(scoreRecord);

    scoreRecord.attach(dataSheetView3);
    scoreRecord.attach(dataSheetView5);
    scoreRecord.attach(minMaxView);

    addScores(scoreRecord);

    scoreRecord.detach(dataSheetView3);
    StatisticsView statisticsView = new StatisticsView(scoreRecord);
    scoreRecord.attach(statisticsView);

    addScores(scoreRecord);
  }
}
```

위 코드는 새로운 성적 출력 기능을 추가할 때 내부 코드를 수정하지 않고 외부에서 attach와 detach 메서드를 호출하여 추가, 삭제한다. - Subject라는 클래스를 만들어서 여러 개의 Observer 클래스들을(성적 출력 객체) 관리하고 있다.

addScore 메서드를 사용하여 성적을 추가하면 notifyObserver 메서드를 호출하여 observers 변수에 추가했던 객체들을 loop 돌면서 update 메서드를 호출하여 최신 결과를 출력한다. (Subject 클래스인 ScoreRecord를 정의할 때 Observer 클래스에 의존하지 않는 방식으로 설계해야 한다.)

<a href="http://www.yes24.com/Product/Goods/108192370">헤드 퍼스트 디자인 패턴</a>
