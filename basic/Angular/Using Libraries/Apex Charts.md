### Step 1

```javascript
npm install ng-apexcharts
```

### Step 2

```javascript
//inside of a feature module
//shared.module.ts
import { NgApexchartsModule  } from 'ng-apexcharts';
...
imports: [
	NgApexchartsModule
]
...
```

### Step 3

> Create a component simple-pie-chart.component.ts

### Step 4

```html
//simple-pie-chart.component.html
<div id="chart">
    <apx-chart
      [series]="chartOptions.series"
      [chart]="chartOptions.chart"
      [labels]="chartOptions.labels"
      [responsive]="chartOptions.responsive"
    ></apx-chart>
</div>
```

### Step 5

```javascript
//simple-pie-chart.component.ts
import { Component, ViewChild, Input } from '@angular/core';
import { ChartComponent, ApexNonAxisChartSeries, ApexResponsive, ApexChart } from 'ng-apexcharts';

export type ChartOptions = {
  series: ApexNonAxisChartSeries;
  chart: ApexChart;
  responsive: ApexResponsive[];
  labels: any;
};

@Component({
  selector: 'app-simple-pie-chart',
  templateUrl: './simple-pie-chart.component.html',
  styleUrls: ['./simple-pie-chart.component.css']
})
```

### Step 6

```javascript
//simple-pie-chart.component.ts
export class SimplePieChartComponent {

  @ViewChild("chart") chart: ChartComponent;
  public chartOptions: Partial<ChartOptions>;

  @Input() series = [50, 50];
  @Input() width = 380;
  @Input() labels = ["A", "B"];

  constructor() {
    this.chartOptions = {
      series: this.series,
      chart: {
        width: this.width,
        type: "pie"
      },
      
      labels: this.labels,
      responsive: [
        {
          breakpoint: 480,
          options: {
            chart: {
              width: 200
            },
            legend: {
              position: "bottom"
            }
          }
        }
      ]
    };

  }

}
```

### Step 7

```html
//app.component.html
<app-simple-pie-chart>
</app-simple-pie-chart>
<router-outlet></router-outlet>
```

![[Pasted image 20230909195447.png]]

