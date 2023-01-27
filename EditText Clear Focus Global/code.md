# EditText 바깥 아무데나 클릭시 Focus Clear

## Activity의 dispatchTouchEvent를 override 하기

```
override fun dispatchTouchEvent(ev: MotionEvent?): Boolean {
        if (ev?.action != MotionEvent.ACTION_DOWN) { return super.dispatchTouchEvent(ev) }
            
        val cf = currentFocus
        if (cf !is EditText) { return super.dispatchTouchEvent(ev) }
        
        val outRect = android.graphics.Rect()
        cf.getGlobalVisibleRect(outRect)
        if (!outRect.contains(ev.rawX.toInt(), ev.rawY.toInt())) {
            cf.clearFocus()
            val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
            imm.hideSoftInputFromWindow(cf.windowToken, 0)
        }

        return super.dispatchTouchEvent(ev)
    }
```

- base activity 같은 곳에 override 할 경우 모든 액티비티(프래그먼트)에서 적용된다

### Reference
- https://notepad96.tistory.com/entry/Android-EditText-remove-focus
