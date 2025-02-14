from phi.agent import Agent
from phi.model.groq import Groq
from phi.tools.yfinance import YFinanceTools
from dotenv import load_dotenv
import os

# Load environment variables
load_dotenv()finance_agent = Agent(
    name="Financial AI Agent",
    model=Groq(id="llama-3.3-70b-versatile"),
    tools=[YFinanceTools(
        stock_price=True,
        analyst_recommendations=True,
        stock_fundamentals=True,
        company_news=True,
        technical_indicators=True,
        historical_prices=True,
    )],
    instructions=["Use tables to display the data"],
    show_tool_calls=True,
    markdown=True,
)import yfinance as yf
import matplotlib.pyplot as plt

def visualize_stock_data(ticker):
    stock_data = yf.download(ticker, start="2024-01-01", end="2025-02-14")
    plt.figure(figsize=(10, 5))
    plt.plot(stock_data['Close'], label='Close Price')
    plt.title(f'{ticker} Stock Price')
    plt.xlabel('Date')
    plt.ylabel('Price')
    plt.legend()
    plt.show()

# Call the function
visualize_stock_data("AAPL")multi_ai_agent = Agent(
    model=Groq(id="llama-3.3-70b-versatile"),
    team=[web_search_agent, finance_agent],
    instructions=["Always include sources", "Use tables to display the data"],
    markdown=True,
    debug_mode=True,
) 
