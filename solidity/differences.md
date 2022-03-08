# Solidity база знаний 1

### Получение адреса контракта
- (from v5) this become deprecated. You now have to use `address(this)`
- (v5 and older) http://solidity.readthedocs.org/en/latest/frequently-asked-questions.html#get-contract-address-in-solidity \
`this` is a variable representing the current contract. Its type is the type of the contract. Since any
 contract type basically inherits from the address type, this is always convertible to address and in this
 case contains its own address.