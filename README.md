# How to create a real time Chart (SfChart) using MVVM in WPF

This demo explains how to create a real time [chart in WPF](https://help.syncfusion.com/wpf/charts/getting-started).

The following code example illustrates how to update the **ItemsSource** bound to chart series in real time use case.

**[XAML]**

```
<Window.DataContext>
        <local:DataGenerator/>
</Window.DataContext>

<Grid>
        <chart:SfChart x:Name="Chart" Margin="0,20,20,0" AreaBorderThickness="0,1,1,1">
            <chart:SfChart.PrimaryAxis>
                <chart:DateTimeAxis LabelFormat="hh:mm:ss">
                    <chart:ChartAxis.Header>
                        <TextBlock Margin="10" Text="Time" FontSize="16" FontFamily="SegoeUI"/>
                    </chart:ChartAxis.Header>
                </chart:DateTimeAxis>
            </chart:SfChart.PrimaryAxis>

            <chart:SfChart.SecondaryAxis>
                <chart:NumericalAxis Minimum="1000" Maximum="1006" 
                                     Interval="1" >
                    <chart:ChartAxis.Header>
                        <TextBlock Margin="10" Text="Value" FontSize="16" FontFamily="SegoeUI"/>
                    </chart:ChartAxis.Header>
                </chart:NumericalAxis>
            </chart:SfChart.SecondaryAxis>

            <chart:FastLineBitmapSeries EnableAntiAliasing="False" 
                                        XBindingPath="Date" YBindingPath="Value" 
                                        StrokeThickness="1"
                                        ItemsSource="{Binding DynamicData}"/>
            <chart:FastLineBitmapSeries EnableAntiAliasing="False" 
                                        XBindingPath="Date" YBindingPath="Value1" 
                                        StrokeThickness="1"
                                        ItemsSource="{Binding DynamicData}"/>
            <chart:FastLineBitmapSeries EnableAntiAliasing="False" 
                                        XBindingPath="Date" YBindingPath="Value2" 
                                        StrokeThickness="1"
                                        ItemsSource="{Binding DynamicData}"/>
        </chart:SfChart>
</Grid>
```
**[C#]**
```
public class Data
{
        public Data(DateTime date, double value, double value1, double value2)
        {
            Date = date;
            Value = value;
            Value1 = value1;
            Value2 = value2;
        }

        public DateTime Date { get; set; }

        public double Value { get; set; }

        public double Value1 { get; set; }

        public double Value2 { get; set; }
}

public class DataGenerator
{
        public int DataCount = 50000;
        private int RateOfData = 5;
        private ObservableCollection<Data> Data;
        private Random randomNumber;
        int myindex = 0;
        DispatcherTimer timer;

        public ObservableCollection<Data> DynamicData { get; set; }

        public DataGenerator()
        {
            randomNumber = new Random();
            DynamicData = new ObservableCollection<Data>();
            Data = new ObservableCollection<Data>();
            Data = GenerateData();
            LoadData();

            timer = new DispatcherTimer();
            timer.Tick += timer_Tick;
            timer.Interval = new TimeSpan(0, 0, 0, 0, 50);
            timer.Start();
        }

        public void AddData()
        {
            for (int i = 0; i < RateOfData; i++)
            {
                myindex++;
                if (myindex < 100)
                {
                    DynamicData.Add(this.Data[myindex]);
                }
                else if (myindex > 100)
                {
                    DynamicData.RemoveAt(0);//Remove data not visible
                    DynamicData.Add(this.Data[(myindex % (this.Data.Count - 1))]);
                }
            }
        }

        public void LoadData()
        {
            for (int i = 0; i < 10; i++)
            {
                myindex++;
                if (myindex < Data.Count)
                {
                    DynamicData.Add(this.Data[myindex]);
                }
            }
        }

        public ObservableCollection<Data> GenerateData()
        {
            ObservableCollection<Data> datas = new ObservableCollection<Data>();

            DateTime date = new DateTime(2009, 1, 1);
            double value = 1000;
            double value1 = 1001;
            double value2 = 1002;
            for (int i = 0; i < this.DataCount; i++)
            {
                datas.Add(new Data(date, value, value1, value2));
                date = date.Add(TimeSpan.FromSeconds(5));

                if ((randomNumber.NextDouble() + value2) < 1004.85)
                {
                    double random = randomNumber.NextDouble();
                    value += random;
                    value1 += random;
                    value2 += random;
                }
                else
                {
                    double random = randomNumber.NextDouble();
                    value -= random;
                    value1 -= random;
                    value2 -= random;
                }
            }

            return datas;
        }

        private void timer_Tick(object sender, EventArgs e)
        {
            AddData();
        }
}

```
## Output:

![Real timeline Chart in WPF](https://user-images.githubusercontent.com/53489303/200725932-8b21e2ce-d5b3-451a-917f-b68b1886b692.gif)

KB article - [How to create a real time Chart (SfChart) using MVVM in WPF](https://www.syncfusion.com/kb/11416/how-to-create-a-real-time-chart-sfchart-using-mvvm-in-wpf)

## See also

[How to bind a simple collection to Chart (SfChart) in WPF](https://help.syncfusion.com/wpf/charts/databinding#binding-a-simple-collection-to-the-chart)

[Tips to improve the performance of Chart in WPF](https://help.syncfusion.com/wpf/charts/performance)

[What are the available types of Chart](https://help.syncfusion.com/wpf/charts/series)
