---
layout: post
title:  "Parsing Nu Invest Broker Notes"
date:   2022-01-18 11:19:17 -0300
smmry: Writing the stock transactions on a spreadsheet is error-prone. So, I've created an online app to write the transactions for me. I just copy and paste them.
read_time: 3 min
---

I've always liked the financial market. I even took a course during college about it. Later, when I took a Python course, the example of a chart was a Markowitz efficient frontier. I was just thrilled. Yet I don't buy or sell stocks frequently: I'm more of a holder. Unfortunately, I don't calculate valuations or Sharpe ratios. It's delightful to determine the potential of each company. But that takes a whole lot of time. Nowadays, I follow a research house. They call the shots and, I just read their recommendations and operate the broker.

<div align="center">
  <img alt="Hodor holding the door" style="width:600px" src="{{site.url}}/assets/img/broker_note/hodor.jpeg"/>
  <br/><small>Hold</small><br/><br/>
</div>

# About Taxes

The Brazilian income tax declaration is a PITA. Some information is missing in the exchange platform for people who started investing before 2018 (maybe 2017). I have bought some stocks before that, so I can't just copy and paste my position from the exchange site. So, I made a financial spreadsheet to keep track of all my information to scratch my itch. It calculates the quantity and the average cost of each stock.

Writing a lot of entries to a spreadsheet is ridiculously error-prone. So I found a Python library that parses pdfs into plain text. It's not perfect: it generates a lot of extra end of lines. But regex made it (kind of) easy to extract the exact information I cared about. Boom, I had a simple Python script capable of extracting data from broker notes (Nu Invest exchange). I used the script in a Jupyter Notebook or in a shell for two months or so.

# Coding

The app should be pretty straightforward. So a micro framework would suffice. I chose to work with [Flask](https://flask.palletsprojects.com/en/2.0.x/) due to its simplicity. Its documentation is well written, and it has the majority of topics I need to create and test the application. Although, I employed an extra reference about [testing](https://medium.com/analytics-vidhya/how-to-test-flask-applications-aef12ae5181c).

The back-end dependencies were Flask and Fitz (the pdf reading library). On the front-end, it seemed that every dependency created an additional level of complexity to the tests, like:

- How to test react-redux action-dispatchers/reducers?
- How to test pure React components?
- How to test React-Router-Dom?
- What the hell is enzyme?
 
I got one answer at a time and, [Testing Library documentation](https://testing-library.com/docs/example-react-redux/) helped me a lot.

It was time to deploy the application. I needed a platform with a free tier and easily configurable: Heroku fits both requirements. Using the [Heroku tutorial for python](https://devcenter.heroku.com/articles/getting-started-with-python) and the [static buildpack](https://elements.heroku.com/buildpacks/heroku/heroku-buildpack-static), I could deploy the app without difficulties.

It's easy to use, drag and drop a broker note from Nu Invest and hit Enviar. A new line will show containing: transaction data, ticker, buy (C) or sell (V), quantity, stock price, total transaction value with taxes, the broker (Nu Invest) and, the note identification number.

Buy is “C” and sell is “V” because the site is in Portuguese (oh, really?). Buy = Comprar and Sell = Vender. Also, the floating points are comma-separated (that's how we use them in Brazil).

<div align="center">
  <img alt="Broker note parser" style="width:800px" src="{{site.url}}/assets/img/broker_note/nuinvestparser.png"/>
  <br/><small>Example of output by the parser</small><br/><br/>
</div>

It was a great experience and also was the first app I created. Even though I work every day in different areas of a web app, I've never had to develop one from scratch. That's when some challenges emerge, like serving the front-end from a back-end route in development and production.

If you are interested, here is the [GitHub Repo](https://github.com/lucaslazzaris/nuinvest_parser). Or if you have some feedback, please contact me: <a href="mailto:lucascercal.l@gmail.com">lucascercal.l@gmail.com</a>.