- #+BEGIN_QUERY
  {:title "List of Tags containing 'semester' (Sorted)"
   :query [:find (pull ?t [*])
           :where
           [?b :block/tags ?t]
           [?t :block/name ?name]
           [(clojure.string/includes? ?name "semester")]
   ]
   :view (fn [rows]
     (let [sorted-rows (sort-by (fn [r]
                                  (let [n (get r :block/name "")]
                                    ;; Sort by Length first, then Alphabetically.
                                    ;; This makes 'Semester 1' (short) come before 'Semester 10' (long)
                                    [(count n) n]))
                                rows)]
       [:table [:thead [:tr [:th "Tag Name"]]]
        [:tbody (for [r sorted-rows]
                  [:tr [:td [:a {:href (str "#/page/" (get-in r [:block/name]))}
                                (get-in r [:block/original-name])]]])]]))
  }
  #+END_QUERY
-