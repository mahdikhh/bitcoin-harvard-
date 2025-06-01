# bitcoin-harvard
#محاسبه کردن قیمت بیت کوین در به صورت آنلاین و دقیق


import sys
import requests


def main():
    # If no command-line argument is provided, ask the user for input
    bitcoins = get_bitcoin_count()

    # Fetch the current Bitcoin price in USD
    try:
        price_per_bitcoin = fetch_bitcoin_price()
    except requests.RequestException:
        sys.exit("Error: Could not retrieve Bitcoin price.")

    # Calculate the total cost
    total_cost = bitcoins * price_per_bitcoin

    # Output the total cost in USD to four decimal places
    print(f"${total_cost:,.4f}")


def get_bitcoin_count():
    """Get the number of Bitcoins to buy from the user or command-line argument."""
    if len(sys.argv) == 2:
        try:
            return float(sys.argv[1])
        except ValueError:
            sys.exit("Error: NUMBER_OF_BITCOINS must be a valid number.")
    else:
        while True:
            try:
                bitcoins = float(input("Enter the number of Bitcoins you want to buy: "))
                return bitcoins
            except ValueError:
                print("Please enter a valid number.")


def fetch_bitcoin_price():
    """Fetch the current price of Bitcoin in USD."""
    url = "https://api.coindesk.com/v1/bpi/currentprice.json"
    response = requests.get(url)
    response.raise_for_status()  # Raise an error for unsuccessful requests

    # Parse the JSON response
    data = response.json()

    # Extract the USD price as a float
    return data["bpi"]["USD"]["rate_float"]


if __name__ == "__main__":
    main()
