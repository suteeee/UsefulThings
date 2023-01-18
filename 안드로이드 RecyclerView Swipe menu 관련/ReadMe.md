# 사용법

## 1. Recycler View Item Layout xml 생성
- 생성할 때 background layout과 foreground layout이 필요하다.
- ex) <FrameLayout> 안에 <ConstraintLayout> <ConstraintLayout> 두개가 match_parent, match_parent 형태로 겹쳐져 있는 상태

## 2. Swipe Helper Callback 을 Override하는 클래스 생성
- getView(recycierView: RecyclerView) 만 재정의 하면 된다.
- (recyclerview.adapter as [해당 어댑터 뷰홀더]).binding.foregroundLayout 을 리턴으로 주면 된다.

## 3. Recycler View 있는곳으로 가서 Helper를 attach 하기
- 2번에서 생성한 Helper 클래스 생성 후 setClamp() 사용해서 swipe시 멈출 구간 정해야 한다.
  ```
  val helper = MySwipeHelper().apply { setClamp(100f) }
  ```
- swipeHelper를 인자로 전달해 ItemTouchHelper 생성후 recycler view에 attach 하기
  ```
  ItemTouchHelper(helper).attachToRecyclerView(recyclerView)
  ```
  
## 4. Recycler View 다른 아이템 / 빈공간 클릭시 열려있는 아이템 닫히게 touch listener 추가하기
- touch listener 생성하기
  ```
  val touchListener: View.OnTouchListener = View.OnTouchListener {  v, _ ->
    helper.removePreviousClamp(this)
    v.performClick()
    false
  }  
  ```
- touch listener 붙히기
  ```
  recyclerView.setOnTouchListener(touchListener)
  ```

