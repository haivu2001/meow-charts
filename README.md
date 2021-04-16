Meow-charts
----
An easy-to-use charting library for Svelte applications.

## Column chart example

```sveltehtml

<ColumnChart items={[45,61,33,75,11,55,98]}
             fill="blue"
             xLabels={["Mon","Tue","Wed","Thu","Fri","Sat","Sun"]}
             yLabels={["0%","25%","50%","75%","100%"]} formatter={(x,y) => `It's ${y}% done as of ${x}.`}
             columnColors={["red","orange","pink","purple","yellow","darkred","brown"]}
             width="100%" height="100%"/>
```