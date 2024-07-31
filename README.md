I think that this is a good example of structured concurrency, the use of Task with async/await.

The goal here is to load tabs and their respective data. Once their data is available, the tab is shown. The alternative would be to try showing the tabs ahead of time, and then load in content. If content is not available, show a progress view.

So the options look like:
<ul>
<li> show tabs dynamically </li>
    <ul>
    <li> preload concurrently, then fix order afterwards ([video](dynamic-tab-preload-concurrently.mov)) </li>
    <li> load one at a time, in order ([video](dynamic-tab-in-order.mov)) </li>
    </ul>
<li> show tabs initially </li>
    <ul>
    <li> preload concurrently, so content is there when selecting tab ([video](initial-tab-preload-concurrently.mov)) </li>
    <li> load once tab is selected, or onAppear ([video](initial-tab-load-onappear.mov)) </li>
    </ul>
</ul>

The set up and response times are not that great for these requests currently. So it is pretty noticeable when you await all tabs in order. So instead I offer a concurrent approach where they're allowed to come in ASAP, but are then synchronized afterwards to the originally intended order.