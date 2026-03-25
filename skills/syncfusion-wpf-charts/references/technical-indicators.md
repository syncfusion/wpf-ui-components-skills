# Technical Indicators

Technical indicators help analyze financial and stock market data by identifying trends, momentum, and volatility patterns.

## Overview

SfChart supports 10+ technical indicators for financial analysis:

- Simple Moving Average (SMA)
- Exponential Moving Average (EMA)
- Triangular Moving Average (TMA)
- Relative Strength Index (RSI)
- MACD (Moving Average Convergence Divergence)
- Bollinger Bands
- Accumulation Distribution
- Average True Range (ATR)
- Momentum
- Stochastic

## Simple Moving Average (SMA)

Calculate average over a specified period.

```xml
<syncfusion:SfChart>
    <!-- Candlestick Series -->
    <syncfusion:CandleSeries ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"/>
    
    <!-- SMA Indicator -->
    <syncfusion:SimpleAverageIndicator ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"
                             Period="14"
                             SignalLineColor="Blue"/>
</syncfusion:SfChart>
```

**Period:** Number of data points to average (default: 14)

## Exponential Moving Average (EMA)

Gives more weight to recent prices.

```xml
<syncfusion:ExponentialAverageIndicator ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         High="High"
                         Low="Low"
                         Open="Open"
                         Close="Close"
                         Period="14"
                         SignalLineColor="Red"/>
```

## Triangular Moving Average (TMA)

Double-smoothed moving average.

```xml
<syncfusion:TriangularAverageIndicator ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         High="High"
                         Low="Low"
                         Open="Open"
                         Close="Close"
                         Period="14"
                         SignalLineColor="Green"/>
```

## Relative Strength Index (RSI)

Measures momentum and overbought/oversold conditions (0-100 scale).

```xml
<syncfusion:RSITechnicalIndicator ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         High="High"
                         Low="Low"
                         Open="Open"
                         Close="Close"
                         Period="14"
                         SignalLineColor="Purple"/>
```

**Interpretation:**
- Above 70: Overbought
- Below 30: Oversold

## MACD (Moving Average Convergence Divergence)

Shows relationship between two moving averages.

```xml
<syncfusion:MACDTechnicalIndicator ItemsSource="{Binding StockData}"
                          XBindingPath="Date"
                          High="High"
                          Low="Low"
                          Open="Open"
                          Close="Close"
                          ShortPeriod="12"
                          LongPeriod="26"
                          SignalLineColor="Blue"
                          ConvergenceLineColor="Red"
                          DivergenceLineColor="Green"/>
```

**Periods:**
- **Short:** Fast EMA (default: 12)
- **Long:** Slow EMA (default: 26)

## Bollinger Bands

Show volatility bands around a moving average.

```xml
<syncfusion:BollingerBandIndicator ItemsSource="{Binding StockData}"
                                   XBindingPath="Date"
                                   High="High"
                                   Low="Low"
                                   Open="Open"
                                   Close="Close"
                                   Period="20"
                                   UpperLineColor="Red"
                                   LowerLineColor="Red"
                                   SignalLineColor="Blue"/>
```

**StandardDeviation:** Number of standard deviations for bands (default: 2)

## Accumulation Distribution

Measures cumulative money flow.

```xml
<syncfusion:AccumulationDistributionIndicator ItemsSource="{Binding StockData}"
                                              XBindingPath="Date"
                                              High="High"
                                              Low="Low"
                                              Open="Open"
                                              Close="Close"
                                              SignalLineColor="Orange"/>
```

## Average True Range (ATR)

Measures market volatility.

```xml
<syncfusion:AverageTrueRangeIndicator  ItemsSource="{Binding StockData}"
                         XBindingPath="Date"
                         High="High"
                         Low="Low"
                         Open="Open"
                         Close="Close"
                         Period="14"
                         SignalLineColor="Brown"/>
```

## Momentum Indicator

Shows rate of price change.

```xml
<syncfusion:MomentumTechnicalIndicator ItemsSource="{Binding StockData}"
                              XBindingPath="Date"
                              High="High"
                              Low="Low"
                              Open="Open"
                              Close="Close"
                              Period="14"
                              SignalLineColor="Teal"/>
```

## Stochastic Indicator

Compares closing price to price range over time.

```xml
<syncfusion:StochasticTechnicalIndicator ItemsSource="{Binding StockData}"
                                XBindingPath="Date"
                                High="High"
                                Low="Low"
                                Open="Open"
                                Close="Close"
                                KPeriod="14"
                                DPeriod="3"
                                SignalLineColor="Blue"
                                UpperLineColor="Red"
                                LowerLineColor="Green"/>
```

## Multiple Indicators Example

Combine indicators for comprehensive analysis:

```xml
<syncfusion:SfChart>
    <!-- Primary chart with candlestick -->
    <syncfusion:CandleSeries ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"
                             YAxis="{Binding ElementName=primaryAxis}"/>
    
    <!-- Bollinger Bands -->
    <syncfusion:BollingerBandIndicator ItemsSource="{Binding StockData}"
                                       XBindingPath="Date"
                                       High="High"
                                       Low="Low"
                                       Open="Open"
                                       Close="Close"
                                       Period="20"
                                       YAxis="{Binding ElementName=primaryAxis}"/>
    
    <!-- SMA -->
    <syncfusion:SimpleAverageIndicator ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"
                             Period="50"
                             SignalLineColor="Blue"
                             YAxis="{Binding ElementName=primaryAxis}"/>
    
    <!-- RSI on separate axis -->
    <syncfusion:RSITechnicalIndicator ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"
                             Period="14"
                             YAxis="{Binding ElementName=rsiAxis}"/>
    
    <!-- Axes -->
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis x:Name="primaryAxis"/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis x:Name="rsiAxis"
                                  OpposedPosition="True"
                                  Minimum="0"
                                  Maximum="100"/>
    </syncfusion:SfChart.SecondaryAxis>
</syncfusion:SfChart>
```

## Common Properties

All indicators share these properties:

- **ItemsSource:** Data collection
- **XBindingPath:** X-axis property (typically Date)
- **High, Low, Open, Close:** Price properties
- **Period:** Calculation period (days)
- **SignalLineColor:** Line color
- **YAxis:** Associated Y-axis

## Indicator Selection Guide

| Indicator | Purpose |
|-----------|---------|
| SMA/EMA/TMA | Trend identification |
| RSI | Overbought/oversold conditions |
| MACD | Momentum and trend changes |
| Bollinger Bands | Volatility and price ranges |
| Stochastic | Entry/exit signals |
| ATR | Volatility measurement |
| Momentum | Rate of price change |
| Accumulation Distribution | Money flow analysis |

## Data Requirements

Financial series require specific properties:

```csharp
public class StockData
{
    public DateTime Date { get; set; }
    public double Open { get; set; }
    public double High { get; set; }
    public double Low { get; set; }
    public double Close { get; set; }
    public double Volume { get; set; }  // Optional
}
```

## Styling Indicators

Customize indicator appearance:

```xml
<syncfusion:SimpleAverageIndicator  SignalLineColor="Blue"
                         StrokeThickness="2"
                         Opacity="0.8"/>
```
