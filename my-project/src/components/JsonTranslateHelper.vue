<template>
  <div class="hello">
	
	  <div class="container is-fluid">
		  <div class="notification">
			
			
			  <div class="columns">
				  <div class="column">
					  <div class="file">
						  <label class="file-label">
							  <input class="file-input" type="file" name="resume" accept=".json"  @change="processFile($event)">
							  <span class="file-cta">
				      <span class="file-icon">
				        <i class="fas fa-upload"></i>
				      </span>
				      <span class="file-label">
				        Choose a file…
				      </span>
				    </span>
						  </label>
					  </div>

					  <button type="button" class="button is-success is-outlined" @click="getLanEnData">LOAD 'lang-en' FROM SERVER</button>
					  <button type="button" class="button is-success is-outlined" @click="getFaqEnData">LOAD 'faq-en' FROM SERVER</button>
					  
					  <button type="button" class="button is-success is-outlined" @click="createOriginalData">Create</button>
					  
					  <div v-if="typeFile !== ''">You uploaded <b>{{typeFile}}</b> original file</div>
				  </div>
				  <div class="column">
					  <button type="button" class="button is-success is-outlined" @click="downloadFiles">Download result</button>
				  </div>
				  <div class="column">
					  <button type="button" class="button is-outlined" @click="createJsonText">Create JSON Code</button>
				  </div>
			  </div>
		  </div>
	  </div>
	
	  <!--<div class="container is-fluid">
			  <div class="columns table is-bordered">
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Original</h2>
					  </div>
				  </div>
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Your translate</h2>
					  </div>
				  </div>
				  <div class="column">
					  <div class="notification">
					    <h2 class="subtitle">Result</h2>
					  </div>
				  </div>
			  </div>
	  </div>-->
	
	  <div class="container is-fluid">
		  <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
			  <thead>
			    <tr>
				    <th style="width:10%">Original</th>
				    <th style="width:40%">Your translate</th>
				    <th style="width:50%">Result</th>
			    </tr>
			  </thead>
			  <tbody id="tbody">
			  
			  </tbody>
			  
			  
			 <!-- <div class="column">
					  <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth" id="original-text">Original</table>
			  </div>
			  <div class="column">
					  <div id="translate-text"></div>
			  </div>
			  <div class="column">
					  <table id="result-text"></table>
			  </div>-->
		  </table>
	  </div>
	
	  <div class="container is-fluid">
		  <h3>Result:</h3> <p>Wrap line?<input type="checkbox" @click="wrapToggle"/></p>
		  <code v-bind:style="{ whiteSpace: wrap }">{{translateText}}</code>
	  </div>
  </div>
</template>

<script>
const faqUrlFile = [{
    1: {
        type: 'full',
        title: 'What is VogoV and what is OGO coin?',
        textShort:'<p>VogoV is a decentralized interactive adult film studio located in Los Angeles comprising VogoV.com and VogoV.studio, which is a platform that allows users to both vote on the production...</p>',
        text: `<p>VogoV is a decentralized interactive adult film studio located in Los Angeles comprising VogoV.com and VogoV.studio, which is a platform that allows users to both vote on the production of adult videos as well as discuss the progress of the studio.</p>

    <p>OGO coin is the service-use token of VogoV. The coin provides three benefits:</p>
    <ol>
    <li>Access to VogoV.com is three times cheaper using OGO coin when compared to using fiat currency. During the first five months after the token sale ends, a three-month membership will only cost two OGO coin tokens.</li>
    <li>
    A unified payment method within the adult entertainment industry based on partnerships and a liquidity pool which will accelerate token adoption and provide the following benefits to token holders:
    <ol>
    <li>Access to VogoV’s anchor partners like TeenMegaWorld.net and TmwVRnet.com is three times cheaper when compared to using fiat currency.</li>
    <li>Discount on paid memberships at other partners.</li>
    <li>Special exclusive offerings from us and our partners only for OGO coin.</li>
    </ol>
    </li>
    <li>An opportunity to take part in the decision-making process regarding the production of adult videos on VogoV.studio platform. A token holder has a right to vote on scenarios, performers, and locations. Besides voting, the token holder can also participate in open discussions where one can give feedback on produced videos and share one’s thoughts and ideas.</li>
    </ol>
    <p>On top of OGO coin’s benefits, we are developing a cryptocurrency payment infrastructure called OgoShift for the whole adult industry in order to simplify and proliferate cryptocurrency usage beyond just OGO coin:</p>
    <ol>
    <li>he infrastructure is comprised of an exchange, a marketplace, and a merchant account. These are all interconnected with one another by our cryptocurrency wallet for a seamless user experience when using various cryptocurrencies as a payment method. The infrastructure is specifically tailored to become an important part of the $100 billion adult entertainment industry.</li>
    <li>From a business perspective, ecosystem growth is supported by partnerships with top industry players. These players have already started the tokenization of their in-platform currencies.</li>
    </ol>`
    },
    2: {
        type: 'basic',
        title: 'When can I use your tokens to buy something? Where will they be accepted?',
        text: `<p>We have already developed and integrated our payment system for OGO coin acceptance into our partners’ websites.</p>

    <p>You can buy subscriptions to VogoV.com, TeenMegaWorld.net and TmwVRnet.com immediately after you buy our tokens.</p>`
    },
    3: {
        type: 'basic',
        title: 'What is your token price?',
        text: `<p>The token price is 0.002 ETH or 1 ETH for 500 OGO coins. At this moment, the token price of OGO coin in USD is equal to <span="usd-count">0.002</span>, according to ETH’s market price to USD on CoinMarketCap.</p>`
    },
    4: {
        type: 'full',
        title: 'Can I buy OGO coin with fiat currencies?',
        textShort:'<p>This option is available only during the private sale and the second round of the crowdsale, which can be cancelled. In an effort to be as transparent as possible...</p>',
        text: `<p id="test">This option is available only during the private sale and the second round of the crowdsale, which can be cancelled. In an effort to be as transparent as possible, we are holding our first crowdsale round on a smart contract, which can only accept ETH.</p>
    <p>If you would like to buy ETH, here’s a short guide:</p>
    <ol>
    <li>
    <p class="title">Register at an exchange</p>
    <p class="text">Register at your chosen exchange by submitting a few personal details. Full identity checks are often included later in the process when a deposit or withdrawal is made.</p>
    </li>
    <li>
    <p class="title">Complete KYC/identity checks</p>
    <p class="text">Before or after depositing, or prior to withdrawal, exchanges must carry out Know Your Customer (KYC) and Anti-Money Laundering (AML) checks. Exchanges will require proof of address and photo identification to proceed.</p>
    </li><li>
    <p class="title">Choose a deposit method</p>
    <p class="text">Each Ethereum exchange will offer their own banking methods. These are often a mix of bank wire transfers, SEPA, credit/debit card or PayPal payments. Each exchange will typically charge a fee for each deposit method; fee details are usually found in the footer of the exchange’s website.</p>
    </li><li>
    <p class="title">Make a deposit in US dollars, Euros, etc.</p>
    <p class="text">Deposits will take from as little as 24 hours to several days to arrive in your exchange account. Deposit times will vary from exchange to exchange and the deposit method chosen.</p>
    </li><li>
    <p class="title">Buy Ether with your deposited funds</p>
    <p class="text">Once your fiat currency has arrived in your exchange account, you can use this currency to purchase Ether. Beginner friendly platforms such as Coinbase have made this process very simple. Exchanges which make purchases easy are usually marked with a tick symbol under “Beginner Friendly”.</p>
    </li><li>
    <p class="title">Withdraw the Ether to your wallet</p>
    <p class="text">Once you have bought Ether, remember to make sure the coins are sitting in your own personal wallet. This is relevant mainly when buying Ether from an exchange. If you leave your coins on the exchange and the exchange gets hacked or becomes insolvent, you may end up losing your coins.</p>
    </li>
    </ol>
    <p class="attention"><span>NB!</span> Please don’t send your ETH from exchange wallets; if you do, you’re going to lose your ETH.<br /> Please first send your ETH to your personal wallet that’s compatible with unlisted ERC-20 tokens.</p>
    <p class="attention"><span>NB!</span> If you are using any other wallet than MEW or Metamask, please make sure that your wallet<br /> is compatible with unlisted ERC-20 tokens.</p>
    <p class="c">Read more about some other ways to buy ETH: <br /><a target="_blank" href="https://99bitcoins.com/guide-buy-ether-ethereum/">7 Simple ways to Buy Ethereum Instantly (Credit card, Wire or Cash) | 99Bitcoins</a></p>`
    },
    5: {
        type: 'full',
        title: 'What problems does VogoV solve?',
        textShort:'<p>First, the adult entertainment industry is experiencing a kind of stagnation at this moment. What has been working up to now is becoming a thing of the past. Cryptocurrency acceptance as...</p>',
        text: `<p>First, the adult entertainment industry is experiencing a kind of stagnation at this moment. What has been working up to now is becoming a thing of the past. Cryptocurrency acceptance as a payment method is only one facet of this. Cryptocurrency and the blockchain can be used for much more than just a payment method. We’re using cryptocurrency to change the way we interact with our community of fans.</p>
    <ol>
    <li>
    The first step is to develop a decentralized interactive adult video studio where fans will be able to vote on what they really want to watch. First, they complete a survey which is used to create several scenarios based on voters’ preferences. After the survey, the scenarios are formed and voted on. At this stage, the voters determine the direction of the studio’s video production.
    </li>
    <li>
    The second step is to implement an interactive business model, which will include us as the producers of adult videos, our membership subscribers and our voters. At this moment, it’s quite hard for fans of adult videos to find videos they really want since they’re faced with a ton of different videos. We plan to implement a smart chatbot designed with the help of a group of sexologists to help an individual find what they want. The chatbot will also gather depersonalized and completely anonymous data about what our watchers want. After it’s automatically analyzed and some tendencies are found, the voters can decide what content will be produced, taking into account the preferences and desires of the fans who are not voters. At this stage, the watchers (members/fans) determine the direction for the video production and the voters correct the direction.
    </li>
    </ol>
    <p>Second, the problem of anonymous and secure payment methods with low fees is solved by OGO coin and cryptocurrencies in general. </p>
    <p>Third, the absence of a unified adult-industry cryptocurrency supported and adopted by industry players who make a difference can be solved by OGO coin.</p>
    <p>Fourth, OgoShift can address some of the problems the adult industry faces that have not yet been solved through the use of cryptocurrency. For example, it’s vital for adult businesses to accept recurring fixed-amount and pay-per-view payments. This is what OgoShift solves through the off-chain customer-to-merchant payments within the infrastructure and the on-chain payments through smart contracts using ETH.</p>
    <p>Fifth, cryptocurrency can be difficult to understand and use for the average person. The marketplace in concert with the benefits provided by OGO coin can significantly help proliferate and simplify cryptocurrency usage. This will have a favorable impact on the cryptocurrency market cap as a whole since some percentage of the $100 billion industry will move into cryptocurrency.</p>
    `
    },
    6: {
        type: 'full',
        title: 'What value can your porn-star advisors deliver to the project except public attention and marketing promotion?',
        textShort:'<p>One of our co-founders made the strong point that a lot of ICOs just pay their advisors or work with them only for crowdfunding purposes. For these ICOs, an advisory board is like a label ...</p>',
        text: `<p>One of our co-founders made the strong point that a lot of ICOs just pay their advisors or work with them only for crowdfunding purposes. For these ICOs, an advisory board is like a label or sign to show investors that the project looks OK and nothing more. To some extent, we view these types of arrangements with advisors as unethical. We support the practice where ICOs describe what advisors do for them. </p>
    <p>Bridgette B advises us on how to attract more industry stars in order to create a pool of performers whom our voters can choose during the voting process.</p>
    <p>Rocco Siffredi has vast industry experience and advises us on video production and studio management matters.</p>
    <p>Our adult-industry advisors are not brand ambassadors. Our advisors will be our advisors regardless of our token sale.</p>
    `
    },
    7: {
        type: 'full',
        title: 'Is there any lockup period?',
        textShort: `<p>No, we don’t see any reason for doing this if we want to be honest with our community. We can pretend to lock up some tokens in a “Founder’s budget”, but this will not stop anyone from having access to the other budgets if they want to sell...</p>`,
        text: `<p>No, we don’t see any reason for doing this if we want to be honest with our community. We can pretend to lock up some tokens in a “Founder’s budget”, but this will not stop anyone from having access to the other budgets if they want to sell some coins on an exchange. We believe in integrity and transparency so we are open with the community.</p>`
    },
    8: {
        type: 'full',
        title: 'Is IKAM Media Inc. the company holding the token sale?',
        textShort: `<p>No, IKAM Media Inc. is the company that will produce videos for VogoV.com on demand. Since we refuse to complete KYC/AML verification, believing this is in the best interests of our token buyers, we have to resort to using a complicated...</p>`,
        text: `<p>No, IKAM Media Inc. is the company that will produce videos for VogoV.com on demand. Since we refuse to complete KYC/AML verification, believing this is in the best interests of our token buyers, we have to resort to using a complicated corporate and banking structure that enables us to exchange ETH to fiat currency and to be compliant.</p>`
    },
    9: {
        type: 'basic',
        title: 'Can I make payments directly from an exchange?',
        text: `<p>Please don’t buy OGO coin from exchange wallets;  if you do, you’re going to lose your ETH. Please first send your ETH to your personal wallet that’s compatible with unlisted ERC-20 tokens.</p>`
    },
    10: {
        type: 'full',
        title: 'Can you clarify how your bonuses work?',
        textShort: `<p>If you had bought our tokens during the private sale, you would have gotten +50% in addition to the amount you had bought. If you buy 15,000 OGO..</p>`,
        text: `<p>If you had bought our tokens during the private sale, you would have gotten +50% in addition to the amount you had bought.</p>
    <p>If you buy 15,000 OGO during the pre-crowdsale, you will receive 20,000 OGO, since 15,000 + 30% = 20,000.</p>
    <p>If you buy OGO coins during the crowdsale, your bonus will depend on the amount of OGO coin tokens that have already been sold. For example, if 6,500,000 OGO were sold, that means the second bonus section is active. In the second bonus, you get a 15% bonus. So, if you buy 10,000 OGO, you will receive 11,500 OGO, since 10,000 + 15% = 11,500.</p>
    `
    },
    11: {
        type: 'basic',
        title: 'After I buy your tokens, where can I store them? Any suggestions?',
        text: `<p>You can use any ERC20 compatible Ethereum wallet such as MetaMask, MyEtherWallet, Ethereum Wallet, Mist, Eidoo, Parity, imToken, Trust, Cipher and more.</p>`
    },
    12: {
        type: 'full',
        title: 'When will OGO coin be listed on exchanges? What exchanges?',
        textShort: `<p>OGO coin will be listed within three months after the first round of the crowdsale ends and within two weeks after the second round. We have already concluded an...</p>`,
        text: `<p>OGO coin will be listed within three months after the first round of the crowdsale ends and within two weeks after the second round. We have already concluded an arrangement with a crypto asset exchange where our token may be traded. However, we are still considering other exchange listing offers we receive. The name/names of the exchange/exchanges will be announced during the crowdsale or earlier.</p>`
    },
    13: {
        type: 'basic',
        title: 'Why will exchange listings happen within three months after the crowdsale?',
        text: `<p>It’s needed to increase the rate of OGO coin acceptance on various adult entertainment websites, which will increase demand.</p>`
    },
    14: {
        type: 'basic',
        title: 'I can’t find your bounty campaign. Do you have one?',
        text: `<p>No, we don’t have one at this moment. However, we may launch a bounty campaign. If we launch it, the bounty campaign budget will be paid from the ecosystem development budget</p>`
    },
    15: {
        type: 'basic',
        title: 'Do you plan any airdrops?',
        text: `<p>We are considering the possibility of an airdroз. It is going to happen in August 2018.</p>`
    },
    16: {
        type: 'full',
        title: 'How can I become waitlisted?',
        textShort: `<p>The following steps will occur during the lottery participation process:...</p>`,
        text: `<p>The following steps will occur during the lottery participation process:</p>
    <ol>
    <li>A potential token buyer gets waitlisted by submitting a waitlist request on the website.</li>
    <li>Three weeks before both the pre-crowdsale and the first crowdsale round start, every waitlisted potential token buyer receives an email inviting them to enter the wallet address from which they are going to send ETH to the smart contract address for either the pre-crowdsale or the first round of the crowdsale. After a potential token holder enters the address, the potential token holder will get an ID. This address registration will continue for four weeks once it starts.</li>
    <li>After the pre-crowdsale and the first crowdsale round end, we will compare the wallet addresses from which OGO coins were bought and the registered addresses. As a result of the comparison, we will determine who the real token buyers are in our wait list.</li>
    <li>Every waitlisted token buyer that actually buys our token receives an email confirming that their ID is participating in the lottery to win a trip to LA for our exclusive party. The waitlisted potential token buyers who didn’t become real token buyers will also receive an email notifying them that their IDs won’t be participating in the lottery. If something goes wrong and we make a mistake, our support will correct it.</li>
    <li>All the participating IDs will be published, so that every waitlisted token buyer can make sure that their ID is participating in the lottery. Within three weeks after the IDs are published, we will use an independent and trustworthy service to determine the winners. We will do our best to make this process as transparent and fair as possible.</li>
    </ol>
    <p>If a lottery winner doesn’t reply to our follow-up emails within 14 days after the winners are announced, the winner’s ID will be invalidated and a new winner chosen. If a winner replies to our email, the winner needs to conform to our information policies, which are similar to Know Your Customer regulations. We need this information to help us prepare accommodation, buy flight tickets and help with getting a US visa, if required. If a winner refuses to provide such data, the winner’s ID will be invalidated and a new winner will be chosen.</p>
    <p>The lottery covers the pre-crowdsale and first round crowdsale participants.</p>
    <p>Only token buyers can win in the lottery.</p>
    `
    },
    17: {
        type: 'full',
        title: 'What is gonzo style in porn?',
        textShort: `<p>There is no sense of telling you a lot about that because you need to see it. You can find gonzo adult videos on VogoV.com or on some porn tubes in...</p>`,
        text: `<p>There is no sense of telling you a lot about that because you need to see it. You can find gonzo adult videos on VogoV.com or on some porn tubes in the gonzo categories.</p>
    <p>In a nutshell, Gonzo pornography takes its cues from gonzo journalism. In gonzo journalism, the reporter does not pretend to be outside of the events being reported and instead is an integral part of them. The same philosophy is used in gonzo pornography, which places the viewer into the actual film. In gonzo pronography, often the actors will not only perform but also film the action. The filmed action will include a lot more close-ups and won’t have the distance between the action and camera that can be seen in traditional pornography.</p>
    `
    },
    18: {
        type: 'full',
        title: 'I don’t know anything about cryptocurrency, but I want to get a discounted price for TMW etc. What should I do?',
        textShort: '<p>If you would like to enjoy a discount on membership, you should buy OGO coin tokens. But before you buy OGO coin, you have to buy ETH. You can read more about some...</p>',
        text: `<p>If you would like to enjoy a discount on membership, you should buy OGO coin tokens. But before you buy OGO coin, you have to buy ETH. You can read more about some ways to buy ETH here: <a target="_blank" href="https://99bitcoins.com/guide-buy-ether-ethereum/">7 Simple ways to Buy Ethereum Instantly (Credit card, Wire or Cash) | 99Bitcoins</a></p>
    <p>We will publish a “how to buy” article before the pre-crowdsale starts. If you would like to gain insight into how the process works, please contact us at <a href="mailto:constantine@vogov.io">constantine@vogov.io</a>. </p>
    `
    },
    19: {
        type: 'full',
        title: 'Will you sell OGO coin directly?',
        textShort: '<p>We will only sell OGO coin during our private sale, pre-crowdsale and crowdsale rounds. After these token sales, we won’t sell OGO coin directly. The secondary market will be...</p>',
        text: `<p>We will only sell OGO coin during our private sale, pre-crowdsale and crowdsale rounds. After these token sales, we won’t sell OGO coin directly. The secondary market will be the only place where OGO coin is traded.</p>
    <p>The pre-crowdsale and the first crowdsale round will be held through the smart contract.</p>
    `,
    },
    20: {
        type: 'full',
        title: 'How can I buy OGO coin?',
        textShort: '<p><p>Before you buy OGO coin, you have to buy ETH. You can read more about some ways to buy ETH here: <a  target="_blank" href="https://99bitcoins.com/guide-buy-ether-ethereum/">7 Simple ways to Buy Ethereum Instantly (Credit card, Wire or Cash) | 99Bitcoins</a>. If you have ETH...</p>',
        text: `<p>Before you buy OGO coin, you have to buy ETH. You can read more about some ways to buy ETH here: <a  target="_blank" href="https://99bitcoins.com/guide-buy-ether-ethereum/">7 Simple ways to Buy Ethereum Instantly (Credit card, Wire or Cash) | 99Bitcoins</a>. If you have ETH, we will publish a guide on how you can buy OGO coin.</p>`
    },
    21: {
        type: 'full',
        title: 'Why didn’t develop your own blockchain? Why did you make it on Ethereum?',
        textShort:'<p>Originally, we were planning on developing our own blockchain, but we decided to use the Ethereum blockchain (ERC20 token standard) to issue OGO coin. Since OGO coin is an ERC20 token...</p>',
        text: `<p>Originally, we were planning on developing our own blockchain, but we decided to use the Ethereum blockchain (ERC20 token standard) to issue OGO coin. Since OGO coin is an ERC20 token, it can be kept in a variety of cryptocurrency wallets that support the ERC20 standard. This will make it easier to use and help it gain acceptance within the cryptocurrency community.</p>
    <p>We have started researching new concepts that would make a huge difference if applied to our own blockchain. We technically conceptualize a possible blockchain in the next generation, but it requires a lot of time and proofs of concept. If we do develop our own blockchain, its release will happen in 1.5 years, according to our time estimate. If we do release it, its cryptocurrency will be offered at a ratio of 3:1 to OGO coins. However, this is just our R&D initiative that may be dismissed.</p>
    `
    },
    22: {
        type: 'full',
        title: 'Zcash, Monero, and Dash give much more anonymity than Ethereum. Why do you use Ethereum blockchain?',
        textShort: '<p>The Ethereum blockchain gives our subscribers the level of anonymity and security that they want. For most of our users, it doesn’t matter...</p>',
        text: `<p>The Ethereum blockchain gives our subscribers the level of anonymity and security that they want. For most of our users, it doesn’t matter whether they use Bitcoin, Monero or Zcash to pay anonymously. They are not looking for full anonymity; therefore, the level of anonymity which Ethereum offers completely satisfies our subscribers’ need.</p>`
    },
    23: {
        type: 'basic',
        title: 'Will I be able to vote for not only actors and actresses, but also categories like MILFs, DP, oral and anal sex…?',
        text: '<p>Yes, in some cases. For example, if you vote for a mature actress, it may be classified in the MILF category by default. If you vote for a threesome, you will also be able to vote for Double Penetration.</p>'
    },
    24: {
        type: 'full',
        title: 'Is there an MVP of voting platform?',
        textShort: '<p>Although the voting platform is still in progress, we have already developed an MVP without reporting and open-discussion features. We will release it before the pre-crowdsale...</p>',
        text: `<p>Although the voting platform is still in progress, we have already developed an MVP without reporting and open-discussion features. We will release it before the pre-crowdsale because it’s a great PR opportunity for us. We will let our community take part in the decision-making process of VogoV’s video production even before the crowdsale starts. When the voting ends, the video will be shot and edited within several months.</p>`
    },
    25: {
        type: 'full',
        title: 'Why are you developing a voting platform that’s not built on a smart contract? You claim it has to be decentralized.',
        textShort: '<p>If it were built on smart contracts, it would have been handicapped. Any transaction would require GAS, and would need to be two transactions</p>',
        text: `<p>If it were built on smart contracts, it would have been handicapped. Any transaction would require GAS, and would need to be two transactions: a vote sending OGO coin to a smart contract address and OGO coin being sent back to the voter. Plus such a dApp would have several critical bottlenecks. In other words, we would have sacrificed convenience and common sense in order to proudly claim that we have developed a dApp.</p>`
    },
    26: {
        type: 'full',
        title: 'What about exchange fees?',
        textShort: '<p>If it were built on smart contracts, it would have been handicapped. Any transaction would require GAS, and would need to be two transactions</p>',
        text: `<p>At first, the exchange will be integrated with another exchange that has a huge trade volume. Therefore, the exchange fees will depend on the fees of the other exchange. However, thanks to our business arrangement, we’re able to offer fees that are lower than the fees that are charged on our partner’s exchange. After our exchange becomes an “adult”, we’ll charge the lowest possible fees that still enable us to continue development of the exchange. The exchange is only a part of our business and mainly serves purposes other than just profit generation. The exchange is a complement to our business model, not the whole business model itself.</p>`
    },
    27: {
        type: 'full',
        title: 'I don’t understand why your exchange is needed. There are too many exchanges.',
        textShort: '<p>If it were built on smart contracts, it would have been handicapped. Any transaction would require GAS, and would need to be two transactions</p>',
        text: `<p>If we were about to bring just another exchange onto the market without having some particular reason or tech breakthrough behind it, it would definitely make no sense or any difference. We see the pain point specifically within the adult entertainment industry from a business perspective as well as from a customer perspective. We did research on the necessity of the exchange and determined that an exchange needed to be tailored specifically to the adult industry to satisfy its specific needs. Also, the exchange is part of our infrastructure and is being developed to enhance the value that OgoShift will deliver to our token buyers and to the adult entertainment industry as a whole.</p>`
    },
    28: {
        type: 'basic',
        title: 'Will you start developing the exchange only after the ICO? Will it be built from scratch or will it be bought or licensed?',
        text: '<p>It’s being developed from the ground up. We have already started developing the exchange, but it’s going to be released after our crowdsale.</p>'
    },
    29: {
        type: 'full',
        title: 'What cryptocurrencies will be traded?',
        textShort: '<p>We’ll do our best to list all adult-industry-related cryptocurrencies like BTC, BCH, ETH, ETC, ZEC, XMR, DASH, SPANK,...</p>',
        text: `<p>We’ll do our best to list all adult-industry-related cryptocurrencies like BTC, BCH, ETH, ETC, ZEC, XMR, DASH, SPANK, SXC, TIT, OKO, BUNNY, VIT, OGO, XVG, POT and others. We plan on becoming the place where most of the entertainment-related tokens can be traded.</p>`
    },
    30: {
        type: 'full',
        title: 'How many porn businesses use cryptocurrency? Did you do any market research?',
        textShort: '<p>According to our ongoing market research, we have already found that more than 470 established porn websites, 50 webcams, and 35 sex shops...</p>',
        text: `<p>According to our ongoing market research, we have already found that more than 470 established porn websites, 50 webcams, and 35 sex shops around the globe accept cryptocurrencies, although they mainly only accept Bitcoin. To give a sense of scale, these porn websites get over 4.6 billion visits a month compared to the 2.4 billion visits monthly to PornHub. However, cryptocurrency acceptance and usage is still very low in comparison to peer-to-business payments with fiat currencies. We’re going to solve in order to simplify and proliferate cryptocurrency usage.</p>`
    },
    31: {
        type: 'basic',
        title: 'What do you mean by the interconnected wallet?',
        text: '<p>You will be able to switch from the marketplace to the exchange or vice versa in two clicks. However, it would be more correct to say that it’s an interconnected account rather than a wallet.</p>'
    },
    32: {
        type: 'full',
        title: 'How are you guys different from StacyQ, BunnyToken, Love Network or SpankChain?',
        textShort: '<p>First, our token gives triple value:...</p>',
        text: `<p>First, our token gives triple value: </p>
    <ol>
    <li>cryptocurrency as a secure and anonymous payment method backed by our real and valuable partnerships</li>
    <li>a right to vote for the adult content produced by VogoV.com</li>
    <li>OGO coin gives a 3x discount on VogoV.com, TeenMegaWorld.net, and TMWVRNET.com, all of which have over 5,000 exclusive adult videos</li>
    </ol>
    <p>Second, OGO coin is supported by industry leaders like Rocco Siffredi and Bridgette B who are our anchor advisors. StacyQ, BunnyToken, Love Network and SpankChain don't have as much industry support.</p>
    <p>Third, we're well-known in the industry and have a solid reputation thanks to our founder, Markus Dupree and our partners like TeenMegaWorld with their 15 years of experience in the adult entertainment industry.</p>
    <p>The fourth point is that for our token, OGO coin, we're developing an ecosystem comprised of an adult-industry exchange and marketplace in order to help the adult entertainment industry as a whole move into crypto.</p>
    <p>The fifth point is that we have already developed our service/product and you can try it on VogoV.com. When you buy our tokens during our public sales, you can enjoy their benefits immediately on VogoV, TeenMegaWorld.net, and TmwVRnet.com.</p>

    `
    },
    33: {
        type: 'full',
        title: 'What is the liquidity pool? How does it work together with partnerships?',
        textShort: '<p>In our case, the liquidity pool is US$3,000,000. This money is devoted to OGO coin business-use proliferation. This means that businesses...</p>',
        text: '<p>In our case, the liquidity pool is US$3,000,000. This money is devoted to OGO coin business-use proliferation. This means that businesses offering their services or goods for our tokens will be able to exchange them for USD by contacting us. The liquidity pool is designed to enhance OGO coin’s value as a payment method.</p>'
    },
    34: {
        type: 'full',
        title: 'Will you sell OGO coins directly?',
        textShort: '<p>After the crowdsale, we won’t sell our tokens directly. Instead they will only be available on the secondary market. During the pre-crowdsale and crowdsale...</p>',
        text: `<p>After the crowdsale, we won’t sell our tokens directly. Instead they will only be available on the secondary market. During the pre-crowdsale and crowdsale, our tokens are sold through a smart contract. We only sell our tokens directly to the buyers during the private sale.</p>`
    },
    35: {
        type: 'full',
        title: 'In what circumstances will OGO coin be burned?',
        textShort: '<p>OGO coin will be burned in three cases. The first instance is if we don’t reach the Hard Cap, and all unsold tokens are to be burned. The burn...</p>',
        text: `<p>OGO coin will be burned in three cases. The first instance is if we don’t reach the Hard Cap, and all unsold tokens are to be burned. The burn mechanism also comes into effect if the market token price is lower than its initial crowdsale price. The initial crowdsale price of OGO coin is an average price of 0.002 ETH in USD during the pre-crowdsale and crowdsale period. For example, if 0.002 ETH cost US$1 during the pre-crowdsale and US$2 during the crowdsale, the crowdsale price is US$1.5.</p>
    <p>If the second crowdsale round is renounced, 36,250,000 OGO coin will be burned. The amount burned will equal the whole amount of OGO coin planned to be sold during the second crowdsale round as well as half of the OGO coin budgeted for ecosystem development, the team, the founders, and the company.</p>`
    },
    36: {
        type: 'basic',
        title: 'What is the maximum token supply?',
        text: '<p>The maximum token supply is 75,000,000 OGO coins. But if the second crowdsale round is renounced, 36,250,000 OGO coin will be burned. The amount burned will equal the whole amount of OGO coin planned to be sold during the second crowdsale round as well as half of the OGO coin budgeted for ecosystem development, the team, the founders, and the company.</p>'
    }
}];
const langUrlFile = [{
    "htitle": "VogoV",
    "hwp": "White Paper",
    "hhome": "Home",
    "hfaq": "FAQ",
    "hjoin": "Join Waitlist",
    "hlinkready": "ready to watch",

    "formtitle":"Join waitlist <br>and be&nbsp;sure you won&rsquo;t miss the&nbsp;boat",
    "formname":"Your name *",
    "formnamepl":"Please enter your name",
    "formemail":"Your email *",
    "formhowcoin":"How many ETH worth of OGO coin are you going to buy? *",
    "formanyq": "Do you have any questions?",
    "formtextpl":"If you have any questions, we will be happy to asnwer them. Thank you!",

    "s1title": "<span>Decentralized</span> Porn Studio in&nbsp;Hollywood with&nbsp;a&nbsp;Unified Adult-Industry Cryptocurrency",

    "s1card1": "Participate in&nbsp;studio<br> productions by&nbsp;voting",
    "s1card2": "Enjoy using<br> a&nbsp;unified cryptocurrency<br> and its infrastructure<br> to&nbsp;purchase goods and services",
    "s1card3": "3x&nbsp;cheaper access<br> to&nbsp;the hottest<br> exclusive content",

    "s1timetitle": "PRE-CROWDSALE",
    "s1timedate":"21 - 31 August",
    "s1join":"Join Waitlist to&nbsp;Win Bonus",

    "s2title":"Keep your eye on&nbsp;us",

    "s3marcusdesc":"director &&nbsp;famous male perfomer",
    "s3titlepresents": "PRESENTS",
    "s3titlestudio": "STUDIO",
    "s3titleand": "and",
    "s3titlecoin": "COIN",
    "s3watchvideo": "Watch Video",

    "s3vogovtitle": "is a game-changing way to participate in porn",
    "s3vogovdesc": "VogoV&nbsp;is an&nbsp;already-functioning adult entertainment studio based in&nbsp;Los Angeles that produces and distributes high-quality content in&nbsp;4k&nbsp;and 60fps as&nbsp;well as&nbsp;VR videos. Within our studio, the decision-making process around the production of&nbsp;adult videos is&nbsp;decentralized. This means every token holder can vote on&nbsp;the content being produced. Scenarios, participating actors and actresses, and other aspects of&nbsp;production will be&nbsp;voted on&nbsp;within the voting charter.",
    "s3cards1desc": "&laquo;The Oscars of&nbsp;Porn&raquo;",
    "s3cards2descyear": "<span class='bold'>15 years</span> in the",
    "s3cards2desc5000": "industry &",
    "s3cards2descfilmed": "videos filmed",
    "s3cards3descyear": "<span class='bold'>2 years</span> in VR &</span>",
    "s3cards3filmed": "<span class='bold'>160+</span> videos filmed</span>",
    "s3videoblock2title": "<span class='title'>OGO coin</span> is a ground-breaking way to pay for porn",
    "s3videoblock2desc":"OGO coin is&nbsp;a&nbsp;unified adult-industry cryptocurrency with a&nbsp;$3&nbsp;million liquidity pool to&nbsp;ensure its business-use proliferation. OGO coin is&nbsp;an&nbsp;anonymous and secure way to&nbsp;pay for adult goods and services without intermediaries. To&nbsp;ensure a&nbsp;strong uptake in&nbsp;the use of&nbsp;OGO coin, VogoV is developing the cryptocurrency infrastructure called OgoShift.",
    "s3h4title":"OGO coin can be&nbsp;applied&nbsp;in",
    "s3h4live":"<span class='custom'>Live</span> webcams",
    "s3h4porn":"<span class='custom'>Porn</span> websites",
    "s3h4dating":"<span class='custom'>Dating</span> apps",
    "s3h4online":"<span class='custom'>Online</span> sex shops",

    "s4title": "No need to wait",
    "s4title2": "Enjoy the token&rsquo;s benefits as&nbsp;soon as&nbsp;you buy it",
    "s4desc": "When the crowdsale starts, OGO coin can be&nbsp;used on&nbsp;<span>VogoV.com</span> and its partners immediately",
    "s4money": "$100 billion <span>industry already accepts <span>ogo coin</span></span>",

    "s5title": "Triple Token Value and&nbsp;Economy",
    "s5desc": "OGO coin consists of economic, economical, and&nbsp;community values",
    "s5text1": "<span class='dot'></span><span class='line'></span><span>Community Value:</span> Our token offers token holders the opportunity to&nbsp;take part in&nbsp;the decentralized decision-making process within the production of&nbsp;adult at&nbsp;VogoV.com. Each OGO coin is&nbsp;a&nbsp;vote for the content the token holders really want to&nbsp;watch.",
    "s5text2": "<span class='dot'></span><span class='line'></span><span>Economic Value:</span> OGO coin is&nbsp;a&nbsp;unified adult-industry cryptocurrency. Its market circulation is&nbsp;backed by&nbsp;a&nbsp;$3&nbsp;million liquidity pool and a&nbsp;burn mechanism. The OGO coin will be&nbsp;widely accepted by&nbsp;industry players as&nbsp;a&nbsp;payment method due to&nbsp;our partnership arrangements.",
    "s5text3": "<span class='dot'></span><span class='line'></span><span>Economical Value:</span> Our token provides token holders with membership and pay-per-view access to&nbsp;VogoV.com, TeenMegaWorld.net and TmwVRnet.com that is&nbsp;three times cheaper than paying with fiat currency.",

    "s7title1": "Burn Mechanism",
    "s7desc1": "In&nbsp;order to&nbsp;stabilize the token market price in&nbsp;case it&nbsp;drops lower than its crowdsale price, OGO coins sold back and stored in&nbsp;VogoV wallets are burned.",
    "s7title2": "Liquidity Pool",
    "s7desc2": "OGO coin&rsquo;s acceptance and business-use proliferation is&nbsp;based on&nbsp;strong partnerships and a&nbsp;liquidity pool of&nbsp;$3&nbsp;million. The partners can offer their goods and services for OGO coin and exchange received OGO coins for fiat currencies at&nbsp;VogoV&rsquo;s liquidity pool.",
    "s7count": "liquidity pool",

    "s8title": "Partners",

    "s9title": "OgoShift Payment Infrastructure",
    "s9desc": "Supporting widespread cryptocurrency usage in&nbsp;the adult industry",
    "s9btitle":"Benefits for Businesses",
    "s9b1":"<span>1.</span> Cryptocurrency acceptance without processing fees. Offers fixed-amount, time-recurring and pay-per-view payments",
    "s9b2":"<span>2.</span> Secure finance storage",
    "s9b3":"<span>3.</span> Smooth exchange of crypto and fiat currencies",
    "s9b4":"<span>4.</span> Targeted marketing and distribution channel for services and goods sold for cryptocurrencies",
    "s9ctitle":"Benefits for Customers",
    "s9c1":"<span>1.</span> Easy to&nbsp;purchase adult goods and services with cryptocurrency",
    "s9c2":"<span>2.</span> Convenient, targeted search for goods and services which can be&nbsp;purchased with cryptocurrencies",
    "s9c3":"<span>3.</span> Secure cryptocurrency storage",
    "s9c4":"<span>4.</span> Easy cryptocurrency trading",

    "s10title":"Be&nbsp;Sure You Don&rsquo;t Miss the Boat",
    "s10desc":"Joining the waitlist enters you into a&nbsp;lottery for a&nbsp;chance to&nbsp;win an&nbsp;amazing prize. After the first crowdsale round, VogoV studio is&nbsp;holding an&nbsp;exclusive party in&nbsp;Los Angeles.&nbsp;10&nbsp;waitlisted token buyers will be&nbsp;invited to&nbsp;attend, with free flights and accommodations. It&nbsp;is&nbsp;a&nbsp;great opportunity to&nbsp;meet a&nbsp;ton of&nbsp;popular porn stars and famous producers.",
    "s10btn":"Subscribe",

    "s11title1":"Token Sale Details",
    "s11nfo1":"Token type:",
    "s11nfo2":"Total token supply:",
    "s11nfo3":"Token sale:",
    "s11nfo4":"Token price:",
    "s11info4or":"or",
    "s11nfo5":"1st round Soft Cap:",
    "s11nfo6":"1st round Hard Cap:",
    "s11nfo7":"Unsold tokens:",
    "s11info7desc":"Burned",
    "s11nfo8":"Pre-crowdsale period:",
    "s11nfo8desc":"21 – 31 August, 2018",
    "s11nfo9":"1st round of the crowdsale:",
    "s11nfo9desc":"26 September – 14 November, 2018",
    "s11nfo10":"2st round of the crowdsale:",
    "s11nfo10desc":"26 January – 14 March, 2019",
    "s11nfo11":"Min Transaction Amount:",
    "s11nfo12":"Max Transaction Amount:",
    "s11nfo12desc":"Unlimited",

    "s11title2":"Token Distribution",
    "s11d1":"Token Sale",
    "s11d2":"Ecosystem Dev.",
    "s11d3":"<span class='text'>The company</span>",
    "s11d4":"Founders",
    "s11d5":"Team",
    "s11d6":"<span class='text'>Community campaigns</span>",
    "s11desc":"The pre-crowdsale and the first round of the crowdsale are held on smart contracts, which will be published within three weeks",

    "s12title":"Bonuses",
    "s12desc":"The crowdsale is divided into bonus sections according to the amount of OGO coins sold",
    "s12after":"First Crowdsale Round",

    "s13title":"Crowdsale Success and Implementation",
    "s13desc":"Project implementation depends on&nbsp;the amount of&nbsp;OGO coins sold",
    "s13f1":"Decentralized Interactive Porn Studio, 120+ videos produced",
    "s13f2":"Merchant Account and&nbsp;Wallet, 350+&nbsp;videos&nbsp;produced",
    "s13f3":"Liquidity Pool, <br>450+ videos produced",
    "s13f4":"Exchange & Marketplace, 650+ videos produced",
    "s13f5":"Full Payment Infrastructure, 1000+&nbsp;videos produced",

    "s14title":"Roadmap",
    "s14title1":"2017-2018",
    "s14desc1":"Markus Dupree was the winner<br> of 7 AVN and 1 XBIZ Awards",
    "s14title2":"March 2018",
    "s14desc2":"VogoV.com produced and published 2 excellent<br> videos starring Markus Dupree, Mia Malkova and Gina Valentina",
    "s14title3":"July 2018",
    "s14desc3":"Merchant account MVP integrated with NATS<br>and into VogoV.com, TeenMegaWorld.net,<br>and TmwVRnet.com",
    "s14title4":"August 2018",
    "s14desc4":"Decentralized interactive voting platform<br> demonstration and the first voting session",
    "s14title5":"21 - 31 August 2018",
    "s14desc5":"Pre-crowdsale. OGO coins accepted with a 3x discount<br> at TeenMegaWorld.net and TmwVRnet.com.<br> A 3-month membership for VogoV.com costs<br> only 2 OGO coin",
    "s14title6":"26 September - 14 November  2018",
    "s14desc6":"First Crowdsale Round",
    "s14title7":"November 2018",
    "s14desc7":"Marketplace release",
    "s14title8":"December 2018",
    "s14desc8":"Adoption of the voting charter <span>•</span><br> The first month of voting <span>•</span><br> Production of over 35 videos <span>•</span ",
    "s14title9":"January 2019",
    "s14desc9":" <span>•</span> Exclusive party in Los Angeles<br> <span>•</span> Cryptocurrency wallet and vault release<br> <span>•</span> Over 100 adult entertainment businesses will <br><span class='hide'>•</span> have accepted OGO coin",
    "s14title10":"26 January  - 14 March 2019",
    "s14desc10":"The second round of the crowdsale",
    "s14title11":"March 2019",
    "s14desc11":"Exchange listing",
    "s14title12":"May 2019",
    "s14desc12":"Vogov.com will launch VR adult film production <span>•</span><br> Cryptocurrency wallet, vault and merchant account <span>•</span><br> V2.0 and mobile version release <span class='hide'>•</span>",
    "s14title13":"Q3 2019",
    "s14desc13":" <span>•</span>  Exchange release V1.0<br> <span>•</span>  Over 500 businesses will have accepted OGO coin<br> <span>•</span>  Over 120 videos produced<br> <span>•</span>  Technical investigation into decentralized payment<br><span class='hide'>•</span>   infrastructure development",
    "s14title14":"Q4 2019",
    "s14desc14":"Payment infrastructure goes beyond the adult  <span>•</span><br> entertainment industry <span class='hide'>•</span><br> Over 200 videos produced  <span>•</span>",

    "s14desc":"To be updated",
    "s14after":"<span>VIDEO PRODUCTION GOAL</span> 1000+ high-quality videos",

    "s15title":"Anchor Industry Advisors",
    "s15bridgsoc":"Top Actress",
    "s15bridgdesc":"Bridgette B&nbsp;is&nbsp;a&nbsp;popular actress and model who is&nbsp;also known as&nbsp;the Spanish Doll. Porn sites all over the world list Bridgette B&nbsp;as&nbsp;one of&nbsp;the top porn stars. Bridgette advises VogoV&nbsp;on how to&nbsp;attract more industry stars in&nbsp;order to&nbsp;create a&nbsp;pool of&nbsp;performers whom our voters can choose during the voting process.",
    "s15roccosoc":"Legendary Actor and Director as well as Markus Dupree’s Mentor",
    "s15roccodesc":"Rocco Siffredi is&nbsp;a&nbsp;legend in&nbsp;porn. Everyone who likes porn has seen Rocco Siffredi. He&nbsp;has won over 40&nbsp;AVN awards (&laquo;the Oscars of&nbsp;Porn&raquo;) and has starred in&nbsp;almost 1400 adult films. Rocco Siffredi has vast industry experience and advises&nbsp;us on&nbsp;video production and studio management matters.",

    "s16title": "Team",
    "s16markussoc": "Co-Founder, Director &amp;&nbsp;Male Performer",
    "s16markusdesc": "Markus Dupree is&nbsp;the stage name of&nbsp;Aleksei Maetnyi, a performer who has won 7 AVN awards:",
    "s16markuslist11": "2017-YEAR",
    "s16markuslist12": "Best Double Penetration Sex Scene",
    "s16markuslist13": "Best Orgy/Gangbang Movie",
    "s16markuslist14": "Most Outrageous Sex Scene",
    "s16markuslist21": "2018-YEAR",
    "s16markuslist22": "Male Performer of the Year",
    "s16markuslist23": "Best Anal Sex Scene",
    "s16markuslist24": "Best Group Sex Scene",
    "s16markuslist25": "Best Double Penetration Sex Scene",

    "s16darkkosoc": "Operator",
    "s16darkkodesc": "Jonni Darkko is&nbsp;a&nbsp;well-known figure in&nbsp;the adult entertainment industry and has over 20&nbsp;years of&nbsp;experience as&nbsp;both a&nbsp;former male performer and a&nbsp;director. Having directed over 180 porn videos, he&nbsp;was inducted into the AVN Hall of&nbsp;Fame in&nbsp;2016. Jonni Darkko is&nbsp;in&nbsp;charge of&nbsp;video shooting at&nbsp;VogoV.",

    "s16ivansoc":"Co-Founder &amp;&nbsp;Managing Director",
    "s16vladsoc":"Chief Technical Officer",
    "s16ilyasoc":"Post Production Engineer <br>& Motion Designer",
    "s16nathansoc":"Creative Director",
    "s16pavelsoc":"Senior Front-end <br>& Solidity Developer",
    "s16maksimsoc":"Production Manager",

    "footertitle":"Join us",
    "footerdesc":"TO&nbsp;THE FULLEST EXTENT PERMITTED BY&nbsp;APPLICABLE LAW AND EXCEPT AS&nbsp;OTHERWISE SPECIFIED IN&nbsp;A&nbsp;WRITING BY&nbsp;VOGOV: (I) THE TOKENS ARE SOLD&nbsp;ON AN&nbsp;&laquo;AS&nbsp;IS&raquo; AND &laquo;AS&nbsp;AVAILABLE&raquo; BASIS WITHOUT WARRANTIES OF&nbsp;ANY KIND, AND VOGOV EXPRESSLY DISCLAIMS ALL&nbsp;IMPLIED WARRANTIES AS&nbsp;TO&nbsp;THE TOKENS, INCLUDING, WITHOUT LIMITATION, IMPLIED WARRANTIES OF&nbsp;MERCHANTABILITY, FITNESS FOR A&nbsp;PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT; (II) VOGOV DOES NOT REPRESENT OR&nbsp;WARRANT THAT THE TOKENS ARE RELIABLE, CURRENT OR&nbsp;ERROR-FREE, MEET YOUR REQUIREMENTS, OR&nbsp;THAT DEFECTS IN&nbsp;THE TOKENS WILL&nbsp;BE CORRECTED; AND (III) VOGOV CANNOT AND DOES NOT REPRESENT OR&nbsp;WARRANT THAT THE TOKENS OR&nbsp;THE DELIVERY MECHANISM FOR TOKENS ARE FREE OF&nbsp;VIRUSES OR&nbsp;OTHER HARMFUL COMPONENTS. ",
    "footercopy":"2018 © Copyright Vogov.io All Rights Reserved"
}];

import axios from 'axios';

export default {
  name: 'JsonTranslateHelper',
  data: function () {
    return {
        langFromFile: {},
        faqFromFile: {},

	    originalText: {},
	    translateText: {},
	    originalElement: '',
	    translateElement: '',
	    resultElement: '',
	    tableBody:'',
	    typeFile:'', // site/faq
	    wrap: 'normal'
    }
  },
	mounted: function (  ) {
		this.originalElement = document.querySelector('#original-text');
		this.translateElement = document.querySelector('#translate-text');
		this.resultElement = document.querySelector('#result-text');
		this.tableBody = document.querySelector('#tbody');
	},
	methods: {
		wrapToggle: function ( e ) {
			this.wrap = e.currentTarget.checked ? 'pre-wrap' : 'normal'
		},
		createJsonText: function (  ) {
			this.translateText = '[{';
			
			var i = 0;
			var text;
			var count = Object.keys(this.originalText).length;
			for ( var key in this.originalText )
			{
				text = '';
				text = this.tableBody.querySelector( 'tr.i'+i+' textarea' ).value;
				if ( text !== '' ) console.log(text);
				this.translateText += '"'+key+'": "'+text;
				
				if ( i+1 < count  )
				{
					this.translateText+='",\n';
				}
				
				i++;
			}
			
			
			this.translateText += '}]';
			window.scrollTo(0,document.body.scrollHeight);
		},
		downloadFiles: function() {
			
			this.translateText = '[{';
			
			var i = 0;
			var text;
			for ( var key in this.originalText )
			{
				text = '';
				text = this.tableBody.querySelector( 'tr.i'+i+' textarea' ).value;
				if ( text !== '' ) console.log(text);
				this.translateText += '"'+key+'": "'+text+'",';
				i++;
			}
			
			
			this.translateText += '}]';
			
			
			this.download(this.translateText, this.typeFile+'-translate.json', 'application/json');
		},
		processFile: function(file) {
			let reader = new FileReader();
			let self = this;
			
			
			reader.onload = (e) => {
				if ( eval(e.target.result)[0][1] === undefined )
				{
					this.typeFile = 'lang';
					this.originalText = eval(e.target.result)[0];
				}
				else
				{
					this.typeFile = 'faq';
					this.originalText = eval(e.target.result)[0];
				}
				this.tableBody.innerText = '';
				
				this.createOriginalData();
			}
			reader.onerror = (stuff) => {
				console.log("error", stuff);
				console.log (stuff.getMessage())
			}
			reader.readAsText(event.target.files[0]);
		},
		createOriginalData: function()
		{
			var i = 0;
			for ( var key in this.originalText )
			{
				if ( this.typeFile === 'lang' )
				{
					this.insertElementsLang(key, this.originalText[key],i);
					this.createElementsLang(key, this.originalText[key],i);
				}
				else if ( this.typeFile === 'faq' ) {
					console.log(key, this.originalText[key]);
				}
				i++;
			}
		},
		insertElementsLang: function( title, text, index )
		{
			let parentTREl = document.createElement( 'tr' );
			let textEl = document.createElement( 'td' );
			let textE2 = document.createElement( 'td' );
			let textE3 = document.createElement( 'td' );
			
			textEl.innerText = text;
			parentTREl.classList.add('i'+index);
			textE2.classList.add('t');
			textE3.classList.add('r');
			
			parentTREl.appendChild( textEl );
			parentTREl.appendChild( textE2 );
			parentTREl.appendChild( textE3 );
			
			this.tableBody.appendChild( parentTREl );
		},
		createElementsLang: function( title, text, index )
		{
			let parentTDT = document.querySelector( 'tr.i'+index+' td.t' );
			let parentTDR = document.querySelector( 'tr.i'+index+' td.r' );
			let translate = document.createElement( 'textarea' );
			let result = document.createElement( 'div' );
			
			translate.addEventListener( 'input', (e) => {
				result.innerHTML = e.currentTarget.value;
			});
			
			translate.className = 'textarea';
			result.className = ' is-small';
			
			parentTDT.appendChild( translate );
			parentTDR.appendChild( result );
		},
		download: function(strData, strFileName, strMimeType)
		{
			var D = document,
			    A = arguments,
			    a = D.createElement("a"),
			    d = A[0],
			    n = A[1],
			    t = A[2] || "application/json";
		
			//build download link:
			a.href = "data:" + strMimeType + "charset=utf-8," + escape(strData);
			
			
			if (window.MSBlobBuilder) { // IE10
				var bb = new MSBlobBuilder();
				bb.append(strData);
				return navigator.msSaveBlob(bb, strFileName);
			} /* end if(window.MSBlobBuilder) */
		
		
		
			if ('download' in a) { //FF20, CH19
				a.setAttribute("download", n);
				a.innerHTML = "downloading...";
				D.body.appendChild(a);
				setTimeout(function() {
					var e = D.createEvent("MouseEvents");
					e.initMouseEvent("click", true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
					a.dispatchEvent(e);
					D.body.removeChild(a);
				}, 66);
				return true;
			}; /* end if('download' in a) */
		
		
		
			//do iframe dataURL download: (older W3)
			var f = D.createElement("iframe");
			D.body.appendChild(f);
			f.src = "data:" + (A[2] ? A[2] : "application/octet-stream") + (window.btoa ? ";base64" : "") + "," + (window.btoa ? window.btoa : escape)(strData);
			setTimeout(function() {
				D.body.removeChild(f);
			}, 333);
			return true;
		},
        getLanEnData: function () {
            this.typeFile = 'lang';
            this.tableBody.innerText = '';
            var i = 0;
            for ( var key in this.langFromFile )
            {
				this.insertElementsLang(key, this.langFromFile[key],i);
				this.createElementsLang(key, this.langFromFile[key],i);

                i++;
            }
    	},
        getFaqEnData: function () {
            this.typeFile = 'faq';
            this.tableBody.innerText = '';
            var i = 0;
            for ( var key in this.faqFromFile )
            {
                console.log(this.faqFromFile[key].type);
                if ( this.faqFromFile[key].type === 'basic' )
				{
					//title
                    //this.insertElementsFaq(key, this.faqFromFile[key].title, this.faqFromFile[key].text, i);
                    //this.createElementsFaq(key, this.faqFromFile[key].title, this.faqFromFile[key].text, i);
					//text
                    //this.insertElementsLang(key, this.faqFromFile[key].text,i);
                    //this.createElementsLang(key, this.faqFromFile[key].text,i);
				}
                //this.insertElementsLang(key, this.faqFromFile[key],i);
                //this.createElementsLang(key, this.faqFromFile[key],i);

                i++;
            }
        }
		
	},
    created: function () {
        this.langFromFile = langUrlFile[0];
        this.faqFromFile = faqUrlFile[0];
    }
}
</script>

<style>
	
	body{
		font-family: -apple-system, "Helvetica Neue", "Helvetica", "Arial", "PingFang SC", "Hiragino Sans GB", "STHeiti", "Microsoft YaHei", "Microsoft JhengHei", SimSun, sans-serif;
	}
	.bold
	{
		font-weight: bold;
	}
</style>
