# Search & Highlight

Search a query from a TextView and highlight all matches. The return is a list of indexes of the matches.

E.g. If query is "ic" then ch**ic**ken & p**ic**ture will be highlighted.

```kotlin
private fun filterResults(query: String): List<Int> {
    val searchableTextLowerCase = searchableText.toLowerCase(Locale.ROOT)
    val queryLowerCase = query.toLowerCase(Locale.ROOT)
    var queryLength = 0
    val indexes = mutableListOf<Int>()

    // Searching.
    var index = 0
    while (index != -1) { // O(n) best-case scenario
        index = searchableTextLowerCase.indexOf(queryLowerCase, index + queryLength)
        if (index != -1) {
            indexes.add(index)
        }
        queryLength = query.length
    }

    // Highlighting.
    val spannable = SpannableString(searchableText)
    for (i in indexes) {
        spannable.setSpan(BackgroundColorSpan(YELLOW), i, i + queryLength, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE)
    }
    searchableTextTextView.text = spannable

    return indexes
}
```