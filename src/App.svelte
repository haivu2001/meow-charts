<script lang="ts">
    import ColumnChart from "./ColumnChart.svelte";

    let items = [54, 55, 97, 55, 87, 30, 44, 11, 22]
</script>

<main>
    <ColumnChart items="{items}" gap={5} xOffsets="{[7,0,-7,7,0,-7,7,0,-7]}"
                 columnColors={ ["red", "orange", "blue", "red", "orange", "blue", "red", "orange", "blue"]}
                 xLabels={[null,"Jan",null,false,"Feb",false,false,"Mar",false]}/>
    <ColumnChart items={[345,399,224,200,454,40,477]} fill="darkgreen"
                 xLabels={["Mon","Tue","Wed","Thu","Fri","Sat","Sun"]}
                 max={500}
                 yLabels={[0,50,100,150,200,250,300,350,400,450,500]} formatter={(x) => `${x}cm`} width="460"/>
</main>