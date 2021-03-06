# Lightning receiving browser wallet
A browser wallet for bitcoin that can receive payments on the lightning network via submarine swaps (with help from a lightning service provider)

It's basically the same as [this project](https://github.com/supertestnet/vanilla-js-browser-wallet) except this wallet has a button for receiving sats on lightning.

Big caveat: this wallet is not a *real* lightning wallet. It *fakes it* by trusting a lightning service provider (LSP). The LSP is in a trusted position that some consider custodial, so make sure you trust him or her before using this wallet.

# How to use it

Go here: https://supertestnet.github.io/lightning-receiving-browser-wallet/lnwallet.html

Be aware that it's a weird hybrid of mainnet and testnet, with the lightning part on mainnet. You will lose money if you send real sats to it in its current state because I will keep your mainnet sats and send you testnet sats at almost 1:1 parity. Also, I don't have very many testnet sats, so it will probably break pretty quickly if a lot of people play with it.

Oh yeah and there's no support for *sending* money to lightning invoices, right now you can only *receive* money using a lightning invoice. So don't even try putting a lightning invoice into the "send" tab, it won't do anything because it doesn't even know how to parse a lightning invoice.

# How it works

When you click to receive a lightning payment in this wallet, you'll see a lightning invoice, but it's not *your* lightning invoice. If any payment is sent to that invoice, the LSP can delay it for a few hours or ignore it altogether. But the LSP cannot settle the invoice and take the money -- not without a piece of text that you have. The piece of text is called a preimage.

When someone tries to pay a lightning invoice displayed in this wallet, the LSP detects the attempted payment but can't settle it. Instead, the LSP puts money into a bitcoin address where you can withdraw it if you put the preimage on the blockchain. If you do, the LSP takes the preimage from the blockchain and settles the lightning payment, thus in a sense "reimbursing" themselves. Some people think this counts as non-custodial, but the thing is, this wallet gives the preimage to the LSP *before* the corresponding payment on the base layer gets confirmed. And as any bitcoin developer knows, unconfirmed payments are pretty easy to cancel.

So the LSP could defraud you by canceling the base layer payment after your wallet gives the preimage to the LSP. If they do that then you won't get any money from them but they can still use the preimage you gave them to settle the lightning payment that's supposed to reimburse them for the money they sent to you. They'll still get the reimbursement but they didn't send you anything so it's not really a reimbursement, it's just fraud. That basically means that, in this wallet, you don't immediately have full power over the money people send you via the lightning network. Instead, the LSP has full power over that money, at least until the corresponding payment on the base layer confirms. Some people consider "full power over your money" the same thing as custody of your money.

The LSP shouldn't have that power forever, though, because -- unless he defrauds you -- the payment on the base layer will probably (eventually) get confirmed, and the LSP can't cancel a confirmed payment. So your LSP usually only has a limited window of time during which he or she can defraud you. If you consider the LSP custodial, therefore, it's only *temporary custody* of payments that haven't gotten confirmed yet. Though as any skeptic would quickly point out, "temporary custody" is the same thing as "permanent custody" if the LSP uses it to move the money to a regular bitcoin address that is under the LSP's full control. Because of that risk you should only use this lightning wallet if you trust your LSP.

It's also worth pointing out that the LSP does not have full power over your entire balance in this wallet. They never have control over money you receive without lightning, and when you do receive money via lightning, they only control that payment until it's confirmed -- which shouldn't take very long. Maybe someday I'll make a version of this wallet where it tells you what's going on when you receive a lightning payment and gives you an option to not give your LSP the preimage until the payment is confirmed. If you select that option you would have to wait to receive the money but it would take away the LSP's ability to defraud you so some people might do it.
