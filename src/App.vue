<script setup>
  import { reactive } from "vue";

  import { Metaplex, keypairIdentity, bundlrStorage, toMetaplexFileFromBrowser } from "@metaplex-foundation/js";
  import { Connection, Keypair } from "@solana/web3.js";

  const QUICKNODE_RPC = 'https://dark-autumn-moon.solana-devnet.discover.quiknode.pro/cb18b589f4dc195f3f281d80b3adb841a8af65cf/'
  const connection = new Connection(QUICKNODE_RPC); //конект к сети

  const secret = [90,89,32,181,126,85,91,243,226,106,192,97,91,124,99,199,145,6,122,158,148,244,91,2,8,76,0,231,39,206,226,128,59,35,130,166,24,116,152,8,82,251,235,95,43,86,157,125,23,87,66,157,157,15,165,82,166,174,78,174,144,181,177,43];
  const fromWallet = Keypair.fromSecretKey(new Uint8Array(secret));

  const fileImg = reactive({
    data: null
  })

  let metaplex = Metaplex.make(connection)
    .use(keypairIdentity(fromWallet))
    .use(bundlrStorage({
      address: "https://devnet.bundlr.network",
      providerUrl: QUICKNODE_RPC,
      timeout: 60000,
    }),
  ); // инициализируем metaplex

  const createNFT = async (buffer) => {
 
    const file = await toMetaplexFileFromBrowser(fileImg.data)
    console.log(file);

    const imageUri = metaplex.storage().upload(file); // тут ошибка
    console.log(imageUri);
  }

  const uploadImg = (file) => {
    fileImg.data = file.target.files[0]
  }

</script>

<template>
  <input type="file" name="" id="" @change="uploadImg">
  <button @click="createNFT()">CreateNFT</button>
</template>
