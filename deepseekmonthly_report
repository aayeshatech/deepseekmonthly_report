import streamlit as st
import random
from datetime import date
import pandas as pd

# Constants
NAKSHATRAS = [
    "Ashwini", "Bharani", "Krittika", "Rohini", "Mrigashira", 
    "Ardra", "Punarvasu", "Pushya", "Ashlesha", "Magha", 
    "Purva Phalguni", "Uttara Phalguni", "Hasta", "Chitra", "Swati", 
    "Vishakha", "Anuradha", "Jyeshtha", "Mula", "Purva Ashadha", 
    "Uttara Ashadha", "Shravana", "Dhanishta", "Shatabhisha", "Purva Bhadrapada", 
    "Uttara Bhadrapada", "Revati"
]

ZODIAC_SIGNS = [
    "Aries", "Taurus", "Gemini", "Cancer", "Leo", "Virgo", 
    "Libra", "Scorpio", "Sagittarius", "Capricorn", "Aquarius", "Pisces"
]

PLANETS = ["Sun", "Moon", "Mercury", "Venus", "Mars", "Jupiter", "Saturn", "Rahu", "Ketu"]
ASPECTS = ["Conjunction", "Opposition", "Square", "Trine", "Sextile"]

SECTORS = [
    {"name": "Banking & Finance", "symbols": ["HDFCBANK", "ICICIBANK", "SBIN", "KOTAKBANK", "AXISBANK"], "rulingPlanet": "Jupiter"},
    {"name": "IT", "symbols": ["TCS", "INFY", "WIPRO", "HCLTECH", "TECHM"], "rulingPlanet": "Mercury"},
    {"name": "Automobile", "symbols": ["MARUTI", "TATAMOTORS", "M&M", "BAJAJ-AUTO", "HEROMOTOCO"], "rulingPlanet": "Venus"},
    {"name": "Energy", "symbols": ["RELIANCE", "ONGC", "IOC", "BPCL", "GAIL"], "rulingPlanet": "Sun"},
    {"name": "Pharma", "symbols": ["SUNPHARMA", "DRREDDY", "CIPLA", "LUPIN", "BIOCON"], "rulingPlanet": "Moon"},
    {"name": "Metals", "symbols": ["TATASTEEL", "JSWSTEEL", "VEDL", "HINDALCO", "NMDC"], "rulingPlanet": "Mars"},
    {"name": "FMCG", "symbols": ["HUL", "ITC", "NESTLEIND", "BRITANNIA", "DABUR"], "rulingPlanet": "Venus"},
    {"name": "Infra", "symbols": ["LT", "ADANIPORTS", "ULTRACEMCO", "ACC", "AMBUJACEM"], "rulingPlanet": "Saturn"},
    {"name": "Telecom", "symbols": ["BHARTIARTL", "VODAFONEIDEA", "TATACOMM"], "rulingPlanet": "Rahu"},
    {"name": "Real Estate", "symbols": ["DLF", "SUNTECK", "OBEROIRLTY", "PRESTIGE", "GODREJPROP"], "rulingPlanet": "Ketu"}
]

COMMODITIES = [
    {"name": "Gold", "symbol": "GOLD", "rulingPlanet": "Sun", "global_symbol": "XAUUSD", "global_timing": "04:00-23:00"},
    {"name": "Silver", "symbol": "SILVER", "rulingPlanet": "Moon", "global_symbol": "XAGUSD", "global_timing": "04:00-23:00"},
    {"name": "Crude Oil", "symbol": "CRUDEOIL", "rulingPlanet": "Mars", "global_symbol": "CL1!", "global_timing": "04:00-23:00"},
    {"name": "Bitcoin", "symbol": "BTC-USD", "rulingPlanet": "Uranus", "global_symbol": "BTCUSD", "global_timing": "00:00-24:00"},
    {"name": "Dow Jones", "symbol": "DJIA", "rulingPlanet": "Jupiter", "global_symbol": "YM1!", "global_timing": "18:30-01:00"},
    {"name": "Nasdaq", "symbol": "NASDAQ", "rulingPlanet": "Mercury", "global_symbol": "NQ1!", "global_timing": "18:30-01:00"},
    {"name": "S&P 500", "symbol": "SPX", "rulingPlanet": "Sun", "global_symbol": "ES1!", "global_timing": "18:30-01:00"}
]

# Create lookup dictionaries for faster access
SECTOR_LOOKUP = {sector["name"]: sector for sector in SECTORS}
SYMBOL_TO_SECTOR = {symbol: sector for sector in SECTORS for symbol in sector["symbols"]}
COMMODITY_LOOKUP = {commodity["symbol"]: commodity for commodity in COMMODITIES}
COMMODITY_LOOKUP.update({commodity["global_symbol"]: commodity for commodity in COMMODITIES})

@st.cache_data(show_spinner=False, ttl=3600)
def generate_monthly_report(selected_month, selected_year, report_type, selected_sector=None, selected_symbol=None):
    """Generate monthly astrological trading report with caching"""
    random.seed(f"{selected_month}{selected_year}")
    
    # Calculate days in month more efficiently
    if selected_month == 2:
        num_days = 29 if (selected_year % 4 == 0 and selected_year % 100 != 0) or (selected_year % 400 == 0) else 28
    elif selected_month in [4, 6, 9, 11]:
        num_days = 30
    else:
        num_days = 31
    
    if report_type == "sector":
        sector = SECTOR_LOOKUP.get(selected_sector)
        if not sector:
            return None
            
        report = {
            "type": "sector",
            "month": selected_month,
            "year": selected_year,
            "sector": sector,
            "planetaryTransits": [],
            "keyDates": [],
            "stockRecommendations": []
        }
        
        # Generate transits more efficiently
        for planet in PLANETS:
            if planet != sector["rulingPlanet"] and random.random() < 0.7:
                continue
                
            transit_days = sorted(random.sample(range(1, num_days+1), random.randint(1, 3)))
            for day in transit_days:
                aspects = [
                    {
                        "planet": random.choice([p for p in PLANETS if p != planet]),
                        "type": random.choice(ASPECTS),
                        "nature": "benefic" if random.random() > 0.5 else "malefic"
                    } for _ in range(random.randint(1, 2))
                ]
                
                report["planetaryTransits"].append({
                    "planet": planet,
                    "day": day,
                    "sign": random.choice(ZODIAC_SIGNS),
                    "nakshatra": random.choice(NAKSHATRAS),
                    "aspects": aspects
                })
        
        # Generate key dates more efficiently
        key_days = sorted(random.sample(range(1, num_days+1), random.randint(5, 10)))
        for day in key_days:
            day_transits = [t for t in report["planetaryTransits"] if t["day"] == day]
            
            bullish_factors = sum(
                1 for t in day_transits 
                if any(a["nature"] == "benefic" for a in t["aspects"]) 
                or t["planet"] in ["Jupiter", "Venus"]
            )
            
            bearish_factors = sum(
                1 for t in day_transits 
                if any(a["nature"] == "malefic" for a in t["aspects"]) 
                or t["planet"] in ["Saturn", "Mars", "Rahu", "Ketu"]
            )
            
            if bullish_factors > bearish_factors:
                impact = "bullish"
                action = "Buy"
            elif bearish_factors > bullish_factors:
                impact = "bearish"
                action = "Sell"
            else:
                impact = "neutral"
                action = "Hold"
            
            report["keyDates"].append({
                "day": day,
                "impact": impact,
                "action": action,
                "confidence": random.choice(["High", "Medium", "Low"]),
                "bestTime": random.choice(["Morning", "Midday", "Afternoon"]),
                "transits": [t["planet"] for t in day_transits]
            })
        
        # Generate stock recommendations more efficiently
        bullish_days = [d["day"] for d in report["keyDates"] if d["impact"] == "bullish"] or random.sample(range(1, num_days+1), min(3, num_days))
        
        for symbol in sector["symbols"]:
            entry_day = random.choice(bullish_days)
            
            report["stockRecommendations"].append({
                "symbol": symbol,
                "action": "Buy",
                "entryDay": entry_day,
                "exitDay": min(entry_day + random.randint(1, 5), num_days),
                "target": round(random.uniform(3, 8), 2),
                "stoploss": round(random.uniform(1, 3), 2),
                "confidence": random.choice(["High", "Medium", "Low"]),
                "aspects": [f"{random.choice(PLANETS)}-{random.choice(ASPECTS)}" for _ in range(random.randint(1, 2))]
            })
    
    else:  # Symbol-specific report
        sector = SYMBOL_TO_SECTOR.get(selected_symbol)
        commodity = COMMODITY_LOOKUP.get(selected_symbol)
        
        if not sector and not commodity:
            return None
            
        report = {
            "type": "symbol",
            "month": selected_month,
            "year": selected_year,
            "symbol": selected_symbol,
            "sector": sector["name"] if sector else None,
            "commodity": commodity["name"] if commodity else None,
            "rulingPlanet": (sector or commodity)["rulingPlanet"],
            "keyDates": [],
            "historicalPatterns": []
        }
        
        # Generate signals more efficiently
        signal_days = sorted(random.sample(range(1, num_days+1), random.randint(6, 10)))
        
        for i, day in enumerate(signal_days):
            signal_type = "Entry" if i % 2 == 0 else "Exit"
            action = random.choice(["Buy", "Long"]) if signal_type == "Entry" else random.choice(["Sell", "Short", "Book Profit"])
            impact = "bullish" if signal_type == "Entry" else "bearish"
            
            report["keyDates"].append({
                "day": day,
                "signalType": signal_type,
                "action": action,
                "impact": impact,
                "aspects": [f"{random.choice(PLANETS)}-{random.choice(PLANETS)} {random.choice(ASPECTS)}" 
                            for _ in range(random.randint(1, 3))],
                "bestTime": f"{random.randint(9, 15)}:{random.choice(['00', '15', '30', '45'])}",
                "confidence": random.choice(["High", "Medium", "Low"])
            })
        
        # Generate historical patterns more efficiently
        aspect_seed = report["keyDates"][0]["aspects"][0].split()[0] if report["keyDates"] else f"{random.choice(PLANETS)}-{random.choice(PLANETS)}"
        
        for _ in range(random.randint(3, 5)):
            report["historicalPatterns"].append({
                "aspect": f"{aspect_seed} {random.choice(ASPECTS)}",
                "date": f"{random.randint(1, 28)}/{random.randint(1, 12)}/{selected_year - random.randint(1, 5)}",
                "impact": random.choice(["bullish", "bearish"]),
                "priceChange": round(random.uniform(-10, 10), 2),
                "duration": f"{random.randint(1, 5)} days"
            })
    
    return report

def display_report(report, report_type, selected_sector=None, selected_symbol=None):
    """Optimized display function with memoized components"""
    if report_type == "Sector-wise":
        st.subheader(f"📊 {selected_sector} Sector Monthly Analysis")
        
        bullish_days = len([d for d in report["keyDates"] if d["impact"] == "bullish"])
        bearish_days = len([d for d in report["keyDates"] if d["impact"] == "bearish"])
        
        col1, col2, col3 = st.columns(3)
        col1.metric("📈 Bullish Days", bullish_days)
        col2.metric("📉 Bearish Days", bearish_days)
        col3.metric("🪐 Ruling Planet", report["sector"]["rulingPlanet"])
        
        tab1, tab2, tab3 = st.tabs(["Planetary Transits", "Key Trading Dates", "Stock Recommendations"])
        
        with tab1:
            st.markdown("""
                <style>
                    .transit-card {padding: 15px; border-radius: 8px; background-color: #f8f9fa; margin-bottom: 15px; border-left: 4px solid #6c757d;}
                    .transit-card h4 {margin-top: 0; color: #343a40;}
                    .aspect-pill {display: inline-block; padding: 3px 8px; border-radius: 12px; font-size: 0.8em; margin-right: 5px; margin-bottom: 5px; background-color: #e9ecef;}
                    .benefic {border-left: 4px solid #28a745;}
                    .malefic {border-left: 4px solid #dc3545;}
                </style>
            """, unsafe_allow_html=True)
            
            st.write("### Planetary Transits Affecting This Sector")
            for transit in sorted(report["planetaryTransits"], key=lambda x: x["day"]):
                aspects_html = "".join([
                    f"""<span class="aspect-pill {'benefic' if a['nature'] == 'benefic' else 'malefic'}">
                        {transit['planet']}-{a['planet']} {a['type']}
                    </span>""" 
                    for a in transit["aspects"]
                ])
                
                st.markdown(f"""
                    <div class="transit-card {'benefic' if any(a['nature'] == 'benefic' for a in transit['aspects']) else 'malefic'}">
                        <h4>Day {transit['day']}: {transit['planet']} in {transit['sign']} ({transit['nakshatra']})</h4>
                        <div><strong>Aspects:</strong> {aspects_html if aspects_html else 'None'}</div>
                    </div>
                """, unsafe_allow_html=True)
        
        with tab2:
            st.write("### Key Trading Dates with Market Impact")
            df = pd.DataFrame([{
                "Day": day["day"],
                "Impact": day["impact"].capitalize(),
                "Action": day["action"],
                "Confidence": day["confidence"],
                "Best Time": day["bestTime"],
                "Active Planets": ", ".join(day["transits"])
            } for day in report["keyDates"]])
            
            st.dataframe(
                df.style.applymap(
                    lambda val: 'color: green; font-weight: bold' if val == 'Bullish' else 
                              'color: red; font-weight: bold' if val == 'Bearish' else 
                              'color: gray; font-weight: bold',
                    subset=['Impact']
                ),
                use_container_width=True,
                hide_index=True
            )
        
        with tab3:
            st.write("### Stock Trading Recommendations")
            for stock in report["stockRecommendations"]:
                with st.expander(f"📌 {stock['symbol']}: {stock['action']} on Day {stock['entryDay']}"):
                    cols = st.columns(3)
                    cols[0].metric("🎯 Target %", f"+{stock['target']}%")
                    cols[1].metric("🛑 Stoploss %", f"-{stock['stoploss']}%")
                    cols[2].metric("🔍 Confidence", stock["confidence"])
                    
                    st.write("**Key Aspects Triggering Trade:**")
                    for aspect in stock["aspects"]:
                        st.write(f"- {aspect}")
                    
                    st.write(f"**Suggested Exit Day:** {stock['exitDay']}")
    
    else:  # Symbol-specific report
        st.subheader(f"📈 {selected_symbol} Monthly Trading Analysis")
        
        cols = st.columns(2)
        cols[0].write(f"**Symbol Type:** {'Stock' if report['sector'] else 'Commodity'}")
        cols[0].write(f"**{'Sector' if report['sector'] else 'Commodity'}:** {report['sector'] or report['commodity']}")
        cols[1].write(f"**Ruling Planet:** {report['rulingPlanet']}")
        cols[1].write(f"**Analysis Period:** {date(1900, report['month'], 1).strftime('%B')} {report['year']}")
        
        tab1, tab2 = st.tabs(["Trading Signals", "Historical Patterns"])
        
        with tab1:
            st.write("### Daily Trading Signals")
            for day in report["keyDates"]:
                border_color = "#28a745" if day["impact"] == "bullish" else "#dc3545"
                bg_color = "rgba(40, 167, 69, 0.1)" if day["impact"] == "bullish" else "rgba(220, 53, 69, 0.1)"
                
                st.markdown(f"""
                    <div style="padding: 15px; border-radius: 8px; border-left: 4px solid {border_color}; 
                    background-color: {bg_color}; margin-bottom: 15px;">
                        <h4 style="margin-top: 0;">Day {day['day']}: {day['signalType']} Signal ({day['impact']})</h4>
                        <p><strong>Action:</strong> {day['action']} | <strong>Best Time:</strong> {day['bestTime']}</p>
                        <p><strong>Confidence:</strong> <span style="background-color: rgba(0, 0, 0, 0.1); 
                        padding: 2px 8px; border-radius: 4px;">{day['confidence']}</span></p>
                        <p><strong>Key Aspects:</strong></p>
                        <ul>{"".join([f"<li>{aspect}</li>" for aspect in day['aspects']])}</ul>
                    </div>
                """, unsafe_allow_html=True)
        
        with tab2:
            st.write("### Historical Performance During Similar Transits")
            for pattern in report["historicalPatterns"]:
                change_color = "#28a745" if pattern["priceChange"] > 0 else "#dc3545"
                change_icon = "↑" if pattern["priceChange"] > 0 else "↓"
                
                st.markdown(f"""
                    <div style="padding: 15px; border-radius: 8px; background-color: #f8f9fa; margin-bottom: 15px;">
                        <h4 style="margin-top: 0;">{pattern['aspect']}</h4>
                        <p><strong>Date:</strong> {pattern['date']} | <strong>Duration:</strong> {pattern['duration']}</p>
                        <p><strong>Price Change:</strong> <span style="color: {change_color}; font-weight: bold;">
                            {change_icon} {abs(pattern['priceChange'])}%</span></p>
                        <p><strong>Market Impact:</strong> {pattern['impact'].capitalize()}</p>
                    </div>
                """, unsafe_allow_html=True)

def main():
    """Optimized main function with faster loading"""
    st.set_page_config(page_title="Monthly Astro Trading Report", layout="wide")
    
    st.title("📅 Advanced Monthly Astro Trading Report")
    st.markdown("Generate sector-wise or symbol-specific monthly trading reports based on planetary positions and aspects")
    
    # Use session state to maintain selections
    if 'report_type' not in st.session_state:
        st.session_state.report_type = "Sector-wise"
    
    cols = st.columns(2)
    with cols[0]:
        report_type = st.radio(
            "Report Type", 
            ["Sector-wise", "Symbol-specific"], 
            horizontal=True,
            key="report_type_selector"
        )
    
    cols = st.columns(3)
    with cols[0]:
        month = st.selectbox("Month", range(1, 13), format_func=lambda x: date(1900, x, 1).strftime('%B'))
    with cols[1]:
        current_year = date.today().year
        year = st.selectbox("Year", range(current_year - 5, current_year + 1))
    
    if report_type == "Sector-wise":
        with cols[2]:
            selected_sector = st.selectbox("Select Sector", [s["name"] for s in SECTORS])
        selected_symbol = None
    else:
        with cols[2]:
            # Generate these lists once and cache
            if 'all_symbols' not in st.session_state:
                stock_symbols = [s for sector in SECTORS for s in sector["symbols"]]
                commodity_symbols = [c["symbol"] for c in COMMODITIES] + [c["global_symbol"] for c in COMMODITIES]
                st.session_state.all_symbols = sorted(list(set(stock_symbols + commodity_symbols)))
            selected_symbol = st.selectbox("Select Symbol", st.session_state.all_symbols)
        selected_sector = None
    
    if st.button("Generate Monthly Report", type="primary"):
        with st.spinner("Analyzing planetary transits and historical patterns..."):
            report = generate_monthly_report(
                month, year, 
                "sector" if report_type == "Sector-wise" else "symbol",
                selected_sector, selected_symbol
            )
            
            if report:
                st.success(f"Generated {report_type} Monthly Report for {date(1900, month, 1).strftime('%B')} {year}")
                display_report(report, report_type, selected_sector, selected_symbol)
            else:
                st.error("Could not generate report. Please try different parameters.")

if __name__ == "__main__":
    main()
