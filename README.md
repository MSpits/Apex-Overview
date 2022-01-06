# Apex-Overview

## Feature name : Bonding
This feature allows users to purchase OHM at a discounted price, allowing Olympus to acquire its own liquidity and other reserve assets.
The purchased OHM are immediatly staked at the time of purchase and only vest at the end of the terms. 
The assets/SLP are collected by the treasury.
Allowing user to pay in LP at a discounted price allows the OHM treasury to own main of the LP. 
Terms of the bonds (price, expiration, ...) are defined by the guardian.

It works as follow : 
1) Bonds are added and the terms are set by the guardian in the BondDepository contract.
2) User can deposit asset or SLP in the BondDepository contract which is then sent to the Treasury contract. This create a bond-bonder pair stored in the BondTeller contract. The OHM tokens bought are stacked and locked during the vesting time.
3) Once the acquisition time has elapsed, the user can redeem their OHM.
 
<p align="center">
<img src="https://docs.olympusdao.finance/~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-MV4hwONledQK5nEDaUc%2F-MV4i-z3ha2mSACt8QZb%2F-MVA3yum0jyw8iJsdQwP%2Fimage.png?alt=media&token=6fcb32c9-ec07-48f6-b49f-dc3ff4b74fd8" width="800" height="290">
</p>

###  - Contract : BondDepository 
This contract is responsible for the bonds creation by the guardian. The user interact with this contract to buy bonds. 

The main functions of this contract are :
- addBond() : Called by the guardian to create bonds that can be bought by users.
- setTerms() : Called by the guardian to set the terms of a bond.
- deposit() : Called by the users to buy bonds. Once bought the bond info are stored in the bondTeller thanks to the newBond() function of the BondTeller contract.

###  - Contract : BondTeller
This contract is responsible to handle all the bonds bought and the redeem of the OHM token. 

The main functions of this contract are : 
- newBond() : Called by the BondDepository when a bond is bought.
- redeem() : Called by the users at the end of the vesting period to get their OHM tokens bought.
###  - Contract : StandardBondingCalculator
This contract provides price information on the OHM token for the other bonding contracts. Math done in the contract is made for XYK AMM pool (e.g. Sushiswap pool). The XYK AMM equation is defined by : $$(reserve_{token\_1})*(reserve_{token\_2})= K$$. 
The main functions of this contract are :
- getKValue() : Compute the K value of an XYK pool (e.g. Sushiswap pool).
- valuation() : Compute the OHM valuation of an SLP.
- markdown() : Compute the markdown to be applied on a bonding.
