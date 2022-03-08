# Unsigned Integer
uint8         |                 
uint16        |                  
uint32        |                  
uint64        |                  
uint128       |                   
uint256(uint) |                         

# Signed Integer
int8          |       
int16         |        
int32         |        
int64         |        
int128        |         
int256(int)   |              

Operators:

Comparisons: <=, <, ==, !=, >= and >
Bit operators: &, |, ^ (bitwise exclusive or) and ~ (bitwise negation)
Arithmetic operators: +, -, unary -, unary +, *, /, %, ** (exponentiation), << (left shift) and >> (right shift)

# Fixed Point Numbers

ufixedMxN and fixedMxN where M represents the number of bits taken by the type and N represents how many decimal points are available
M must be divisible by 8 and goes from 8 to 256 bits. N must be between 0 and 80, inclusive. ufixed and fixed are aliases for ufixed128x18 and fixed128x18, respectively.

# Address
address: Holds a 20 byte value (size of an Ethereum address).
address payable: Same as address, but with the additional members transfer and send.

Address Literals:
Hexadecimal literals that pass the address checksum test, for example 0xdCad3a6d3569DF655070DEd06cb7A1b2Ccd1D3AF are of address type. Hexadecimal literals that are between 39 and 41 digits long and do not pass the checksum test produce an error.

# Fixed Byte Arrays
The value types bytes1, bytes2, bytes3, …, bytes32 hold a sequence of bytes from one to up to 32.

Operators:

Comparisons: <=, <, ==, !=, >=, > (evaluate to bool) Bit operators: &, |, ^ (bitwise exclusive or), ~ (bitwise negation), << (left shift), >> (right shift) Index access: If x is of type bytesI, then x[k] for 0 <= k < I returns the k th byte (read-only).

# Shifts
x << y is equivalent to the mathematical expression x * 2**y.
x >> y is equivalent to the mathematical expression x / 2**y, rounded towards negative infinity.

# bytes and string as Arrays
Variables of type bytes and string are special arrays. The bytes type is similar to bytes1[], but it is packed tightly in calldata and memory. string is equal to bytes but does not allow length or index access.

Solidity does not have string manipulation functions, but there are third-party string libraries. You can also compare two strings by their keccak256-hash using keccak256(abi.encodePacked(s1)) == keccak256(abi.encodePacked(s2)) and concatenate two strings using bytes.concat(bytes(s1), bytes(s2)).

You should use bytes over bytes1[] because it is cheaper, since bytes1[] adds 31 padding bytes between the elements. As a general rule, use bytes for arbitrary-length raw byte data and string for arbitrary-length string (UTF-8) data. If you can limit the length to a certain number of bytes, always use one of the value types bytes1 to bytes32 because they are much cheaper.

# Functions
Structure
function (<parameter types>) {internal|external|public|private} [pure|constant|view|payable] [returns (<return types>)]

Access modifiers
public - Accessible from this contract, inherited contracts and externally
private - Accessible only from this contract
internal - Accessible only from this contract and contracts inheriting from it
external - Cannot be accessed internally, only externally. Recommended to reduce gas. Access internally with this.f.

# Modifiers

pure for functions: Disallows modification or access of state.
view for functions: Disallows modification of state.
payable for functions: Allows them to receive Ether together with a call.
constant for state variables: Disallows assignment (except initialisation), does not occupy storage slot.
immutable for state variables: Allows exactly one assignment at construction time and is constant afterwards. Is stored in code.
anonymous for events: Does not store event signature as topic.
indexed for event parameters: Stores the parameter as topic.
virtual for functions and modifiers: Allows the function’s or modifier’s behaviour to be changed in derived contracts.
override: States that this function, modifier or public state variable changes the behaviour of a function or modifier in a base contract.

# Data location and assignment behaviour
Data locations are not only relevant for persistency of data, but also for the semantics of assignments:

Assignments between storage and memory (or from calldata) always create an independent copy.

Assignments from memory to memory only create references. This means that changes to one memory variable are also visible in all other memory variables that refer to the same data.

Assignments from storage to a local storage variable also only assign a reference.

All other assignments to storage always copy. Examples for this case are assignments to state variables or to members of local variables of storage struct type, even if the local variable itself is just a reference.

# Abstract Contracts
Contracts that contain implemented and non-implemented functions. Such contracts cannot be compiled, but they can be used as base contracts.

```
pragma solidity ^0.8.0;

contract A {
    function C() returns (bytes32);
}

contract B is A {
    function C() returns (bytes32) { return "c"; }
}
```

# Global Variables- 
abi.decode(bytes memory encodedData, (...)) returns (...): ABI-decodes the provided data. The types are given in parentheses as second argument. Example: (uint a, uint[2] memory b, bytes memory c) = abi.decode(data, (uint, uint[2], bytes))- 

abi.encode(...) returns (bytes memory): ABI-encodes the given arguments
abi.encodePacked(...) returns (bytes memory): Performs packed encoding of the given arguments. Note that this encoding can be ambiguous!
abi.encodeWithSelector(bytes4 selector, ...) returns (bytes memory): ABI-encodes the given arguments starting from the second and prepends the given four-byte selector
abi.encodeCall(function functionPointer, (...)) returns (bytes memory): ABI-encodes a call to functionPointer with the arguments found in the tuple. Performs a full type-check, ensuring the types match the function signature. sultequals abi.encodeWithSelector(functionPointer.selector, (...))
abi.encodeWithSignature(string memory signature, ...) returns (bytes memory): Equivalent to abi.encodeWithSelector(bytes4(keccak256(bytes(signature)), ...)
bytes.concat(...) returns (bytes memory): Concatenates variable number of arguments to one byte array
block.basefee (uint): current block’s base fee (EIP-3198 and EIP-1559)
block.chainid (uint): current chain id
block.coinbase (address payable): current block miner’s address
block.difficulty (uint): current block difficulty
block.gaslimit (uint): current block gaslimit
block.number (uint): current block number
block.timestamp (uint): current block timestamp
gasleft() returns (uint256): remaining gas
msg.data (bytes): complete calldata
msg.sender (address): sender of the message (current call)
msg.value (uint): number of wei sent with the message
tx.gasprice (uint): gas price of the transaction
tx.origin (address): sender of the transaction (full call chain)
assert(bool condition): abort execution and revert state changes if condition is false (use for internal error)
require(bool condition): abort execution and revert state changes if condition is false (use for malformed input or error in external component)
require(bool condition, string memory message): abort execution and revert state changes if condition is false (use for malformed input or error in external component). Also provide error message.
revert(): abort execution and revert state changes
revert(string memory message): abort execution and revert state changes providing an explanatory string
blockhash(uint blockNumber) returns (bytes32): hash of the given block only works for 256 most recent blocks
keccak256(bytes memory) returns (bytes32): compute the Keccak-256 hash of the input
sha256(bytes memory) returns (bytes32): compute the SHA-256 hash of the input
ripemd160(bytes memory) returns (bytes20): compute the RIPEMD-160 hash of the input
ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address): recover address associated with the public key from elliptic curve signature, return zero on error
addmod(uint x, uint y, uint k) returns (uint): compute (x + y) % k where the addition is performed with arbitrary precision and does not wrap around at 2**256. Assert that k != 0 starting from version 0.5.0.
mulmod(uint x, uint y, uint k) returns (uint): compute (x * y) % k where the multiplication is performed with arbitrary precision and does not wrap around at 2**256. Assert that k != 0 starting from version 0.5.0.
this (current contract’s type): the current contract, explicitly convertible to address or address payable
super: the contract one level higher in the inheritance hierarchy
selfdestruct(address payable recipient): destroy the current contract, sending its funds to the given address
<address>.balance (uint256): balance of the Address in Wei
<address>.code (bytes memory): code at the Address (can be empty)-
<address>.codehash (bytes32): the codehash of the Address
<address payable>.send(uint256 amount) returns (bool): send given amount of Wei to Address, returns false on failure
<address payable>.transfer(uint256 amount): send given amount of Wei to Address, throws on failure
type(C).name (string): the name of the contract
type(C).creationCode (bytes memory): creation bytecode of the given contract, see Type Information.
type(C).runtimeCode (bytes memory): runtime bytecode of the given contract, see Type Information.
type(I).interfaceId (bytes4): value containing the EIP-165 interface identifier of the given interface, see Type Information.
type(T).min (T): the minimum value representable by the integer type T, see Type Information.
type(T).max (T): the maximum value representable by the integer type T, see Type Information.

# Type Breakdown
Unit            | Type                 | typed unit                           | Description           
-------------------------------------------------------------------------------
bool            | Boolean              |                                      |          
string          | String               |                                      |          
unicode         | Unicode Literal      |                                      |          
hex             | Hexidecimal Literal  |                                      |          
uint[3]         | Array Literal        |                                      |          
x[start:end]    | Array Slice          |                                      |          
Enum            | Enum                 |                                      |          
uint8           | Unsigned Integer     | 0-255                                |                   
uint16          | Unsigned Integer     | 0-65535                              |                  
uint32          | Unsigned Integer     | 0-4.294967295 * 10**9                |                  
uint64          | Unsigned Integer     | 0-18.446744073709551614 * 10**18     |                  
uint128         | Unsigned Integer     |                                      |                  
uint256(uint)   | Unsigned Integer     |                                      |                  
int8            | Signed Integer       |                                      |                
int16           | Signed Integer       |                                      |                
int32           | Signed Integer       |                                      |                
int64           | Signed Integer       |                                      |                
int128          | Signed Integer       |                                      |                
int256(int)     | Signed Integer       |                                      |          
ufixedMxN       | FixedPoint Number    |                                      |               
fixedMxN        | FixedPoint Number    |                                      |                    
address         | address              |                                      |           
address payable | address              |                                      |          
bytes1          | Fixed Byte Array     |                                      |                 
bytes2          | Fixed Byte Array     |                                      |                 
bytes3          | Fixed Byte Array     |                                      |                 
..., bytes32    | Fixed Byte Array     |                                      |          
bytes           | Dynamic Byte Array   |                                      |          
wei             | ETher Units          |                                      |                               
gwei            | ETher Units          |                                      |                                
ether           | ETher Units          |                                      |          
seconds         | Time Units           |                                      |                                       
minutes         | Time Units           |                                      |                                       
hours           | Time Units           |                                      |                                     
days            | Time Units           |                                      |                                    
weeks           | Time Units           |                                      |                                                                       
blockhash       | Block Property       | (uint blockNumber) returns (bytes32) | hash of the given block when blocknumber is one of the 256 most recent blocks; otherwise returns zero                               
block.basefee   | Block Property       | (uint)                               | current block’s base fee (EIP-3198 and EIP-1559)                                    
block.chainid   | Block Property       | (uint)                               | current chain id                                    
block.coinbase  | Block Property       | (address payable)                    | current block miner’s address                                     
block.difficulty| Block Property       | (uint)                               | current block difficulty                                       
block.gaslimit  | Block Property       | (uint)                               | current block gaslimit                                     
block.number    | Block Property       | (uint)                               | current block number                                   
block.timestamp | Block Property       | (uint)                               | current block timestamp as seconds since unix epoch                                      
gasleft         | Transaction Property | () returns (uint256)                 | remaining gas                             
msg.data        | Transaction Property | (bytes calldata)                     | complete calldata                               
msg.sender      | Transaction Property | (address)                            | sender of the message (current call)                                 
msg.sig         | Transaction Property | (bytes4)                             | first four bytes of the calldata (i.e. function identifier)                              
msg.value       | Transaction Property | (uint)                               | number of wei sent with the message                                
tx.gasprice     | Transaction Property | (uint)                               | gas price of the transaction                                  
tx.origin       | Transaction Property | (address)                            | sender of the transaction (full call chain)                                