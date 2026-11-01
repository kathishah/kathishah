# A hands-on engineer and executive

Twenty-five years of well-architected engineering work, from writing code to advising boards, always starting from what the company needs.

CTO, VP Engineering, fractional CTO, or field engineer embedded with a customer, through [ArjunTech](https://arjuntech.com). The work is solution-oriented and crosses domains, and it rests on the same fundamentals each time: layers that contain complexity, software that stays extensible, maintainable and secure, and nothing more elaborate than the problem needs. The projects below show it in practice across imaging, fitness, home media, cloud operations, trading, household finance and games: clear interfaces between the parts, security trade-offs and failure modes written down, and tests behind the code.

## Public repos

Each began as a personal pain point and kept public: use the apps, hosted versions, or fork, self-host and learn from the code.

- **[sundayalbum](https://github.com/kathishah/sundayalbum)** (consumer imaging): Digitize photo prints with the simplest mechanism - use your phone to take a high resolution image of the print or image of several prints on one page of a physical album. It finds the page, straightens it, splits out each print, removes sleeve glare, fixes orientation and restores faded color. Live at [sundayalbum.com](https://www.sundayalbum.com) as a web app and a free Mac app. The Python and OpenCV pipeline is built as separate steps, so the same code runs from the command line, on AWS Lambda behind Step Functions, and inside the macOS app. The AI steps are optional and fall back to classic OpenCV when no key is present. Serverless AWS CDK backend, Next.js web app and SwiftUI app in one monorepo; source published for reading.
- **[strength](https://github.com/kathishah/strength)** (fitness): a workout tracker that runs fully offline in the browser and can sync to your own AWS account. Plain HTML, CSS and JavaScript with no framework or build step, one event log on the client, a documented sync contract, and a small storage interface so the backend can be swapped (there is a porting guide). The security trade-offs are written down. The tests run offline in CI. [Live demo](https://kathishah.github.io/strength/).
- **[googletv](https://github.com/kathishah/googletv)** (home media): an Android TV screensaver that plays an Apple Photos library from a Mac over SMB. A `PhotoSource` interface separates where photos come from from how they are shown, decoding runs off the main thread, and a native C++ tile decoder handles very large (24 MP and up) HEIC photos. It is read-only by design, the home-network trust assumptions are stated, and it stays one small app until a second one needs shared code.
- **[util](https://github.com/kathishah/util)** (cloud operations): shell and Python tools for auditing and bulk-tagging AWS resources, plus document converters and macOS and network helpers. The AWS tools are safe by default: the audit only reads unless asked to tag, and the bulk tagger previews first and writes only with `--apply`.

My son and I designed together.

- **[asura-hunt](https://github.com/kathishah/asura-hunt)** (games, October 2026 onward): an original Roblox action RPG that my son and I designed together. Pick a path, fight demons from Indian folklore, level up, buy gear and take on a boss in a walled fortress-city. The code is plain Luau synced into Roblox Studio with Rojo, so it lives in git with the map. The server decides damage, XP, gold and saving, tuning numbers sit in config files, and saves never overwrite newer data. CI runs lint, format, headless tests and a build on every pull request. Early days: combat, the shop and the first zone are built, and the boss fight is next.

## Trading and markets (private)

Language models were not trusted to trade directly: at the time they still hallucinated significantly, and a model is non-deterministic by nature. After Opus and Sonnet 4.1, models became accurate enough at writing code to open a better route: using them to turn a trading strategy into a deterministic algorithm that behaves the same way every time. Even so, the approach relied on engineering fundamentals, purchased historical data, rigorous backtests, a review of the tests themselves for erroneous assumptions, and real money behind the result. The work started with hkd-usd and vix-backtest, moved on to daily_return, and the combined learning became the ik_rabbit platform.

- **hkd-usd** (forex, October 2025): a trading strategy based on the spread of the pegged value of the Hong Kong dollar to the US dollar. USD/HKD minute-bar history sits in DuckDB, with a mean-reversion backtesting framework (engine, portfolio, metrics, parameter optimizer, charts).
- **vix-backtest** (volatility, October 2025): a specific implementation of a mean-reversion thesis on the VIX. It enters trades on SVXY and UVXY options when the VIX is high, with options timed to expire in 60 to 90 days based on a backtest of the last ten years. It runs against an Interactive Brokers account: it sets alerts on the VIX entry and exit ranges, detects entry points, enters the trades automatically according to the rules, and sends notifications to a phone through Pushover.
- **daily_return** (options income, December 2025): a daily trading strategy on SPY and QQQ to produce daily income. It holds the ETF and sells same-day calls each day to collect the premium, with variants that skip days with a large overnight gap or exit intraday. Folded into the ik_rabbit backtests.
- **ik_rabbit** (trading platform, December 2025 onward): runs trading bots that buy, sell and hold stocks and ETFs according to defined strategies. Strategies were backtested extensively against historical market data and tuned on those results before the bots executed them live against the Schwab API, with position state reconstructed from the broker's own transaction ledger as the only ground truth. A backtesting engine covers four strategy families (Rabbit, ETF and stock trend, VIX patterns, and equity income with covered-call and collar engines); only strategies that validate graduate to a live bot, which runs as a scheduled AWS Lambda with its credentials in AWS Secrets Manager.

## Household finance (private)

- **financial-analysis** (September 2026 onward): turns raw bank and card statements and investment holdings into categorized, queryable data. Config-driven parsers, transaction classification with recurring-payment detection, an LLM review step, a DuckDB view over the result with saved SQL metrics, an Excel ledger, and a separate portfolio diversification and benchmark analysis. Raw statements are never committed. Questions can be asked in plain English: Claude turns each question into a read-only DuckDB query across transactions, holdings and security classification, and the query is shown before its result. This is text-to-SQL over structured data, not retrieval over documents. Beyond lookup, the analyzed data, standard forecasting models and an LLM combine to build what-if scenarios: for example, how to exit a concentrated stock position while harvesting the most tax benefit, and what the portfolio would look like in two and five years.

## Forked to study or extend

Forks are kept to read and learn from, unmodified, except where noted.

- **[emdy](https://github.com/kathishah/emdy)** (extended): a markdown reader for macOS (Electron, React, TypeScript). Added an adjustable content width, an outline sidebar and Mermaid diagram rendering, and merged upstream releases.
- **[indextank-engine](https://github.com/kathishah/indextank-engine)** and **[indextank-service](https://github.com/kathishah/indextank-service)** (search): the code LinkedIn open-sourced (Apache 2.0, December 2011) two months after acquiring IndexTank, a real-time hosted search service. The engine is a Java search library with facets, custom scoring and autocomplete; the service is the Python and Django platform around it, with the API, back office, account storefront and multitenant index management.
- **[scribe](https://github.com/kathishah/scribe)** (data infrastructure): Facebook's server for aggregating log data streamed in real time from many machines (2012).
- **[wavefile](https://github.com/kathishah/wavefile)** and **[ruby-wav-file](https://github.com/kathishah/ruby-wav-file)** (music): Ruby libraries for reading and writing WAV audio, studied alongside the rbsoundtouch bindings below, out of an interest in music software (2013).
- **[bitpocket](https://github.com/kathishah/bitpocket)** (sync): a two-way rsync-based directory sync that handles deletions properly (2011).
- **[sinatra-book](https://github.com/kathishah/sinatra-book)** (web): the Sinatra tutorial and cookbook, from the time of the Sinatra side projects below (2014).
- **[courses](https://github.com/kathishah/courses)** and **[ProgrammingAssignment2](https://github.com/kathishah/ProgrammingAssignment2)** (data science): course materials and an assignment from the Coursera Data Science and R Programming courses (2014).

## Side projects since 2012 (archived)

- **[rbsoundtouch](https://github.com/kathishah/rbsoundtouch)** (audio): Ruby bindings for the SoundTouch C++ audio library, written with SWIG (2012, unfinished).
- **[marketcap](https://github.com/kathishah/marketcap)** (finance): a stock lookup app on the Yahoo Finance API (2012-2014).
- **[lifejournal](https://github.com/kathishah/lifejournal)** (journaling): a clone of OhLife, the reply-by-email journal (2014-2017).
