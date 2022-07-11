//
// @author LazyBear 
// 
// List of my public indicators: http://bit.ly/1LQaPK8 
// List of my app-store indicators: http://blog.tradingview.com/?p=970 
//
study("Pip collector [LazyBear]", shorttitle="PIPCOLL_LB", overlay=true, precision=6)
src=input(close, title="Source")
tf1=input("D", title="Timeframe 1"), tf2=input("240", title="Timeframe 2"), tf3=input("60", title="Timeframe 3")
lengthCenter=input(50, title="Center EMA Length")
lengthLower=input(20, title="Distance of lower line from center (pips)")
lengthUpper=input(20, title="Distance of upper line from center (pips)")
showBGColor=input(false, title="Background color on all EMA synch?")
pip=syminfo.mintick 
ltfsrc=ema(src, lengthCenter) < src
stfsrc=ema(src, lengthCenter) > src
ltf1=security(tickerid, tf1, ltfsrc), stf1=security(tickerid, tf1, stfsrc)
ltf2=security(tickerid, tf2, ltfsrc), stf2=security(tickerid, tf2, stfsrc)
ltf3=security(tickerid, tf3, ltfsrc), stf3=security(tickerid, tf3, stfsrc)
ctfsrc=ema(src,lengthCenter), ctfsrcl=ctfsrc-lengthLower*pip, ctfsrcu=ctfsrc+lengthLower*pip
long=ltf1 and ltf2 and ltf3 
short=stf1 and stf2 and stf3 
plot(ctfsrc, color=blue, linewidth=2, title="Center EMA")
plot(ctfsrcl, color=red, linewidth=2, title="Lower")
plot(ctfsrcu, color=green, linewidth=2, title="Upper")
inrange(x)=>(x>=low and x<=high)
plotarrow(long and ((src==ctfsrc) or cross(src, ctfsrc) or (inrange(ctfsrc)))?low:na, maxheight=30, title="Buy Arrow",  colorup=lime)
plotarrow(short and ((src==ctfsrc) or cross(src, ctfsrc) or (inrange(ctfsrc)))?-high:na, maxheight=30, title="Buy Arrow",  colordown=red)
bgcolor(showBGColor?(ltf1 and ltf2 and ltf3)?green:(stf1 and stf2 and stf3)?red:black:na, transp=85)
