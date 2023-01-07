# motoko-merkle-tree
A Merkle Tree for Motoko for use with Certified Variables


 **A merkle tree**

 Same as https://github.com/nomeata/motoko-merkle-tree but with reconstruct added

 If you benefit from this added function, please consider donating to help me continue building cool stuff
 All procededs will be locked in 8 year ICP Neurons
 ICP: 8521de510a846b6b20b2d4795630a29f4f937f0dd09b3bd802f3319d6b1aef45
 ETH: 0xeF5f8a19300f85Fe0806cA4816FcB10bDd24b313
 BTC: 3QLgWdjbF1K5j12T2sHqZtzHHLphyZVA8z


 This library provides a simple merkle tree data structure for Motoko.
 It provides a key-value store, where both keys and values are of type Blob.

 ```motoko
 var t = MerkleTree.empty();
 t := MerkleTree.put(t, "Alice", "\00\01");
 t := MerkleTree.put(t, "Bob", "\00\02");

 let w = MerkleTree.reveals(t, ["Alice" : Blob, "Malfoy": Blob].vals());
 ```
 will produce
 ```
 #fork (#labeled ("\3B…\43", #leaf("\00\01")), #pruned ("\EB…\87"))
 ```

 The witness format is compatible with
 the [HashTree] used by the Internet Computer,
 so client-side, the same logic can be used, but note

  * the trees produces here are flat; no nested subtrees
//     (but see `witnessUnderLabel` to place the whole tree under a label).
  * keys need to be SHA256-hashed before they are looked up in the witness
  * no CBOR encoding is provided here. The assumption is that the witnesses are transferred
    via Candid, and decoded to a data type understood by the client-side library.

 Revealing multiple keys at once is supported, and so is proving absence of a key.

 By ordering the entries by the _hash_ of the key, and branching the tree
 based on the bits of that hash (i.e. a patricia trie), the merkle tree and thus the root
 hash is unique for a given tree. This in particular means that insertions are efficient,
 and that the tree can be reconstructed from the data, independently of the insertion order.

 A functional API is provided (instead of an object-oriented one), so that
 the actual tree can easily be stored in stable memory.

 The tree-related functions are still limited, only insertion so far, no
 lookup, deletion, modification, or more fancy operations. These can be added
 when needed.

 [HashTree]: <https://sdk.dfinity.org/docs/interface-spec/index.html#_certificate>
