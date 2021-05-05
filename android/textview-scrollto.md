# TextView - scrollTo()



Get a coordiante of an index in the TextView, then scrollTo() that position by clicking a button.

```kotlin
val layout = someTextView.layout
val lineOfIndex = layout.getLineForOffset(currentIndex)
val x = layout.getPrimaryHorizontal(currentIndex).toInt()
val y = layout.getLineTop(lineOfIndex)
someTextView.scrollTo(x - 25, y - 50)
```



## Bonus

Creating Previous/Next buttons in the app bar to navigate through the search results (list of indexes of all matches). The TextView will scrollTo() the result each time during navigation.

```kotlin
override fun onOptionsItemSelected(item: MenuItem) = when (item.itemId) {
    R.id.prevItem -> {
        if (searchResultIndexes.isNotEmpty()) {
            currentScrollTo--
            if (currentScrollTo < 1) {
                currentScrollTo = searchResultIndexes.size
            }
            val currentIndex = searchResultIndexes[currentScrollTo - 1]

            val layout = someTextView.layout
            val lineOfIndex = layout.getLineForOffset(currentIndex)
            val x = layout.getPrimaryHorizontal(currentIndex).toInt()
            val y = layout.getLineTop(lineOfIndex)
            someTextView.scrollTo(x - 25, y - 50)

            searchResultsTextView.text = "$currentScrollTo/${searchResultIndexes.size}"
        }
        true
    }

    R.id.nextItem -> {
        if (searchResultIndexes.isNotEmpty()) {
            currentScrollTo++
            if (currentScrollTo > searchResultIndexes.size) {
                currentScrollTo = 1
            }
            val currentIndex = searchResultIndexes[currentScrollTo - 1]

            val layout = someTextView.layout
            val lineOfIndex = layout.getLineForOffset(currentIndex)
            val x = layout.getPrimaryHorizontal(currentIndex).toInt()
            val y = layout.getLineTop(lineOfIndex)
            someTextView.scrollTo(x - 25, y - 50)

            searchResultsTextView.text = "$currentScrollTo/${searchResultIndexes.size}"
        }
        true
    }

    else -> super.onOptionsItemSelected(item)
}
```