# Backtest in Quantconnect with Lean

## What is Lean
[Lean Engine](https://github.com/QuantConnect/Lean) is an open-source algorithmic trading engine built for easy strategy research, backtesting, and live trading

Install Lean:
```Shell
pip install lean
```

## How to use Lean
```Shell
Usage: lean [OPTIONS] COMMAND [ARGS]...

  The Lean CLI by QuantConnect.

Options:
  --version  Show the version and exit.
  --verbose  Enable debug logging
  --help     Show this message and exit.

Commands:
  backtest        Backtest a project locally using Docker.
  build           Build Docker images of your own version of LEAN.
  cloud           Interact with the QuantConnect cloud.
  config          Configure Lean CLI options.
  create-project  Alias for 'project-create'
  data            Download or generate data for local use.
  decrypt         Decrypt your local project using the specified...
  delete-project  Alias for 'project-delete'
  encrypt         Encrypt your local project using the specified...
  gui             Work with the local GUI.
  init            Scaffold a Lean configuration file and data directory.
  library         Manage custom libraries in a project.
  live            Interact with the local machine.
  login           Log in with a QuantConnect account.
  logout          Log out and remove stored credentials.
  logs            Display the most recent backtest/live/optimization logs.
  object-store    Interact with the Organization's Local Object Store.
  optimize        Optimize a project's parameters locally using Docker.
  project-create  Create a new project containing starter code.
  project-delete  Delete a project locally and in the cloud if it exists.
  report          Generate a report of a backtest.
  research        Run a Jupyter Lab environment locally using Docker.
  whoami          Display who is logged in.

```


```Shell
lean create-project <project-name>
lean cloud push && lean cloud backtest <project-name>
```

```Shell
# backtest result:
┌────────────────────────────┬──────────────────┬─────────────────────────────┬──────────────┐
│ Statistic                  │ Value            │ Statistic                   │ Value        │
├────────────────────────────┼──────────────────┼─────────────────────────────┼──────────────┤
│ Equity                     │ $101,693.81      │ Fees                        │ -$3.60       │
│ Holdings                   │ $101,427.48      │ Net Profit                  │ $-3.60       │
│ Probabilistic Sharpe Ratio │ 67.609%          │ Return                      │ 1.69 %       │
│ Unrealized                 │ $1,658.07        │ Volume                      │ $99,730.07   │
├────────────────────────────┼──────────────────┼─────────────────────────────┼──────────────┤
│ Total Orders               │ 1                │ Average Win                 │ 0%           │
│ Average Loss               │ 0%               │ Compounding Annual Return   │ 271.993%     │
│ Drawdown                   │ 2.200%           │ Expectancy                  │ 0            │
│ Start Equity               │ 100000           │ End Equity                  │ 101693.81    │
│ Net Profit                 │ 1.694%           │ Sharpe Ratio                │ 8.861        │
│ Sortino Ratio              │ 0                │ Probabilistic Sharpe Ratio  │ 67.609%      │
│ Loss Rate                  │ 0%               │ Win Rate                    │ 0%           │
│ Profit-Loss Ratio          │ 0                │ Alpha                       │ -0.003       │
│ Beta                       │ 0.997            │ Annual Standard Deviation   │ 0.222        │
│ Annual Variance            │ 0.049            │ Information Ratio           │ -14.58       │
│ Tracking Error             │ 0.001            │ Treynor Ratio               │ 1.972        │
│ Total Fees                 │ $3.60            │ Estimated Strategy Capacity │ $56000000.00 │
│ Lowest Capacity Asset      │ SPY R735QTJ8XC9X │ Portfolio Turnover          │ 19.95%       │
└────────────────────────────┴──────────────────┴─────────────────────────────┴──────────────┘
```
