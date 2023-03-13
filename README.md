# Price_Based-ATR_Bands-indicator
 Fixed Price Based ATR Bands indicator For TradingView(Pinescript)

 ### Warning Note:
 It is not for generating buy-sell signals.
 Making a trading decision with this indicator alone can have negative consequences!

 ### Screenshot

 <img alt="ATR-Bands" src="ATR-Bands.png"> </img>

 ### Code

```js
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © GokhanAltun

//@version=5
indicator("ATR Bands", overlay = true)

group_setup = "Setup Settings"
group_tp = "TP Settings"
group_label = "Label Settings"


price = input.price(0.0, title = "Price", confirm = true, group = group_setup)
atr_len = input.int(14, title = "ATR Length", group = group_setup)
atr_x = input.float(1, title = "ATR Multiplier", step = 0.1, group = group_setup)

show_tp_levels = input.bool(false, "Show TP Levels", group = group_tp)
tp_level = input.float(2, title = "TP Level", step = 0.5, group = group_tp)

show_price_labels = input.bool(false, "Show Price Labels", group = group_label)
bar_timestamp = input.time(timestamp('1 Jan 2023 10:00:00'), 'Bar Timestamp', confirm = true, group = group_label)


current_date = timestamp(syminfo.timezone, year, month, dayofmonth, hour, minute, second)
atr_out = (ta.atr(atr_len) * atr_x)
round_price = math.round(price, int(math.log10(1/syminfo.mintick)))

atr_band_up = round_price + atr_out
atr_band_down = round_price - atr_out

tp_band_up = round_price + ((round_price - atr_band_down) * tp_level)
tp_band_down = round_price - ((atr_band_up - round_price) * tp_level)

plot(round_price, title = "Price Line", color = color.blue)
plot(atr_band_up, title = "ATR Band Up", color = color.yellow)
plot(atr_band_down, title = "ATR Band Down", color = color.yellow)

plot(show_tp_levels ? tp_band_up : na, title = "TP Band Up", color = color.green)
plot(show_tp_levels ? tp_band_down : na, title = "TP Band Down", color = color.green)

if current_date == bar_timestamp and show_price_labels
    label.new(bar_index, tp_band_up, text = str.tostring(math.round(tp_band_up, int(math.log10(1/syminfo.mintick)))), color = color.green, textcolor = color.white, style = label.style_label_down)
    label.new(bar_index, atr_band_up, text = str.tostring(math.round(atr_band_up, int(math.log10(1/syminfo.mintick)))), color = color.red, textcolor = color.white, style = label.style_label_down)
    label.new(bar_index, round_price, text = str.tostring(math.round(round_price, int(math.log10(1/syminfo.mintick)))), color = color.blue, textcolor = color.white, style = label.style_label_up)
    label.new(bar_index, atr_band_down, text = str.tostring(math.round(atr_band_down, int(math.log10(1/syminfo.mintick)))), color = color.red, textcolor = color.white, style = label.style_label_up)
    label.new(bar_index, tp_band_down, text = str.tostring(math.round(tp_band_down, int(math.log10(1/syminfo.mintick)))), color = color.green, textcolor = color.white, style = label.style_label_up)
```