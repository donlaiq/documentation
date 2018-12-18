## Example: Sending & Receiving MAM

This example includes a sender and a receiver.  The sender broadcasts a public MAM message, “MAMTEST”.  The receiver listens for the message then deciphers it.

### Sending MAM

First, import the MAM client, mam.client.js, and the asciiToTrytes converter.

```
require('./errorhandling')
// If using babel polyfill library
 global._babelPolyfill = false

const Mam = require('../external/mam.client.js')
const { asciiToTrytes } = require('@iota/converter')
```

Populate the MAM message:
	mamState is the URL to IRI Node.  In this example, DevNet.
	mamType – public, private, or restricted.  In this example, public
	mamSecret – a secret code used for encryption.  In this example, "DONTSHARETHIS"

```
let mamState = Mam.init('https://nodes.devnet.iota.org:443')
const mamType = 'public'
const mamSecret = 'DONTSHARETHIS'
mamState = Mam.changeMode(mamState, mamType, mamSecret)
```


Generate JSON formatted message.  In this example, there are two keys, 'test' and 'ts' for timestamp.

```
let toSend = JSON.stringify({ 'test': 'MAMTEST', 'ts': new Date() })
console.log(toSend)
```

Convert JSON to trytes and create a MAM message

```
const trytes = asciiToTrytes(toSend)
const message = Mam.create(mamState, trytes)
```


Update the MAM state to the state of this latest message

```
mamState = message.state
```
 
Attach the message to the MAM stream in the Tangle

```
try {
    Mam.attach(message.payload, message.address)
    console.log('Attached to tangle!')
```

Note:  Payload and address can be output using console.log

```
    console.log('Payload ', message.payload)
    console.log('Address ', message.address)
  } catch (e) {
    console.log(e)
}
```

Send a notification announcing the MAM channel ID.   

```
if (mamState.channel.start === 1) {
    console.log('\r\nListen to this stream with\n\r\n\r   >  node recIT.js', message.root, '\r\n\r\n')
  } else {
    console.log('\r\nUpdated root: ', message.root, '\r\n')
  }
```


**Example Output**

```
iotahub@u18:~/HMmam/src$ node sendIT.js

{"test":"MAMTEST","ts":"2018-11-10T17:16:23.897Z"}

Attached to tangle! 

Payload: AQFDIZHNAN9LRVZRGDYLVOWZGNZJZEEHUKJCIGIVHTKC9VXUEWDOTWHZYBQKPGUOEAVJTDUQBNQPYIKVRKJLRG9OMLTW99RCUFQAWJBMSPXKNLTW9PNAECXVJWFDGCJUEXSWVEKROXTHNJFRPKMAZFLINGJHWSDUEKT99SUBL9USOHWIMCIHJGSOONX9SSOUXEV9DVJSKIARTXXTPFNDQQSXER9MQYZKS9CMHDBWWUZE9XRWLIEWKOTEGLILQXRNJIDK9JNSEINQJ9LMYHAZYPTKWH9OFYOEIMWSTWVPEVQKPXWBXIJDKMMFTDKVSASOXSJSHMBZFNAGNLSMNIIIPWHRCW9DE9ZBYLK9HQTPO9WZNXD9QNCZHFLYBXWITQSCSRLEFSWNUWZWQJVKM9MXSDETZGPLKMPCZLTRVGAYIYAZTQAIXHTKUAGBROKFSLHKZVXIPA9KLUIOPKGPZQBLLJOBTQUVNPELRKAXPNN9QJHASKGNYGGZXPMSBZGKLVYICXRQRGVLKSWXNHSKEZYAOMAVUVNEBBL9OHDOGHTANQEVDN9HVSIOBMKDAXICEZKUJVFLMEQVEA9HRPUSBSJPECWRU9SPADZSUUFGWIGRYEZBCTMAS9NKTEBVK9EWPAIEWPQSQYGNGPLUHHCGERDAWOSSCASLCONWGWFQVWLCOSDVTUVPSYXRUCYTK99ZABYP9JFMVJTKHOBYW9FTPKGMSJMHNWLDIJZQSLKEENGQWYZDXGMCXPAUECLGFHIXCEFTVWRUNYZPXYYMYSSIDXGYPNJBDE99KLKHNIVMRFHHEHNRZPTBSQRLGIYGEMQHQ9MKOYTRXIARHPD9GSGDBNMXSCEUPUXFIGYJPISVJELTCLRKGKSC9ZKERKPJMHACSGFAUAUNOTU9IGV9ZFBMDJOCUHGCXF9LDABYQHINQIVYQBFKPODBEFGM9PORJWJBNX9CLVEKFIOVKIUGMTUTPINWI9RFZZQJLRSY9YQIQIS9EWZ9RCLFCZCTETPFWNWAQKZNYZJZSKDKMBHEHOSTMQHRTUSFLRPATNT99IOZWETCETCMFFZXCIUSACNWPVZCKBRLDKBSEKWVTVCCWLQDWRICEHDDXQLCHSGTKGUAOB9LMHWWDSEACYWQMZPHYWVFNNSIQXKLMFPR99LABRSYDXHKNOFA9CNKNGNGOIRNXXGGQMLGMIKMLFFU9IOVRU9HNXCDSSFC99XKJSRPE9A9FPUHEBDGCXHZXDEWFDZHBOGHQLKM9AJPC9ZAANZPLGKOUASRETRYXPAQIHPVHD9ILPFXHNNMZ9LZROECNEZUVRFELPFXVVVGDZRMBUYHDNRDUNZFHKKMYTVKJMMLSLQZHMQWVUWYQPAIX9QOOWLJYYUTAYUVXXCYDRQPGUSCXFYNIXABUIUULEQSLUXEPSAUQELGKXVGBHOXMNSYPWOWREGOEIJBPMGPYHXIFSSKXGFJSDCMVDNRWQTGAU9BJYLOAN9RUBKJFQULMCNVZDTVDENUYOXOOOS9JIAKCE9CORSEQUHPPZSMWOJZEJSCHZVQYBIFXKIYZHKMSYKOOFPVZSHMUHR9IMDSBGSQZGIWWS9LLRQHXRFNUGKPVBJS9X9ZZRJNQODVWEUHUUOMYGEGLYDEPIIMMXOCWWYSZPONYBLNOJBHEEUAQI9SUZSXNJZYCBLINVQRP9OAZAINNFLEJIGB9IGGJUISYIWGDUXQPQBLVUX9PKLSJMUOUCKKBOSUIAGC9WHBDYPMOAAFXCEGH9JZGNZVUCMTRIHQY9PQLRAKPFCUQQEYHLAZRQGBMASARBDLRJCILPGMZGEBKIS9XSJHHXSN9FVTOV9ZTQCWJYWIDDLVYBDVKXTE9WBOBBUYQNWUUXFLAGNM9XZXXESGVNQMRNFMXXLXQZCLQMVKCIQTJNRVWELCCIIBJFVVXUNAWWOHNUXADKDSOJHRYRRXPORGDODIO9XYAGMQWPDKNDDZOHILUIDYNKBIZWZLJOKKAIYCKSNFFRKZQUHWVHISNLELJGFGQPMUQVYYNFVTMFFMPJORWYHSBYOYXAT9P9YIEKCDIFF9XQPCOIYBPBQVZNUASUGWU9JHUZHBQZKEJMNIVTKFKOQXSAUGZVVCATNSXNJRDMA9OTEMIDSGMSHCVATOTEHZNXQQEWINQYOXOKKQEYWNUZMCPHAYUOXI9JHDJICLCJKSCICWPJWWJ9TOT9LDYEFVUIHZJVIQFDLJIDIKADDIEWCR9SYFIUCCQBUTPSHSPHQCCQTI9LWLVJGOLTKPLAACAHLOGZFHVCRUAYJBKMQTFBKP9IMWGLDHROLMMWFGEPDOXIMDXDLWPYCBWDKIGFW9SGMZUHLYOES9Q9XFMHUPXZLUZHHSVFLJKXTXGR9IIQHNXEFFJSPCIUQA9EQPXWMLZLVFDQF9EZVLQQJBUDHXHJHJCAEMPRO9CAMCNTXZHLUCNZU9RXZNHZUPBKUFCVHNMDUXE9LUGXFQGQTQLVTRJLXXMPSPOVILCFIGUNOYLZURSIXYFEROEVZGPWRFWONYAYISPNKMDUFUAFVAAGEOZUTLACGYRUNATRNWJQGSKOXURSCRLOJTNQIVHAYHTB9XOIIOTNVIZKKLHOQCLFULNSZBXIRKFPGJP9LWCSYPGQEXRDERQULHKAUKOVYFQQBNHULKWUSFRRCCF9RCRDFAWPAFKUZKJPRTQZOSKJHEBTQPEHNXZTOGTO9WPLGRDNPLITIJPPVLTIIDBEOTXZZFYRMPNUDYOLDFHTXTCALYQLTXTIEAOL9FTZLVQIVYMCBXT99USJPWNDGKGQOPRAUNILDZXHQGMSTUIUAMEGDS9IOGPJTRMUYLESCGDYWVSVYTISIDVPTMQGB9IDIVWTTWOQIYZLVFOSE9OIOBGJCK9NPYDNCJXKJNPUNPEJZRYMWMZ9HCONEPHNCZYKTEPLWXQWQLVLZFAWGJUBWICKGPTZZDSSLUSKZGRJYR9ICLJXA9DZBTLMKHAURFQFBFPJTOGOSRUZAXIJRRH9ARUUMDYWAACBPCJSXCGRKLSRNSGFPMMEF9XJYGLGEIUKLPLGOXZUCWOYXKOSNWRKRAVOEKWSZO9CDRLBBUF9RJDY9BGGQ99AR9XQAHHGTPNLUCGHAZVBJISWWZIOUXFMKFEORRJPR9TPQCJYGZOEXMNDEKRKJLVZZXCADKEYHSGC9CNJMBRUMVABTZUXW9GPHKDZWNSPIWEOXCHVEVGMFFJWTBQM9ITVLTDYSLJVDJHTZFITGSVWYFEUIHHTAIUG9HBGRUNQTSFFNFLNPAXDZSFNKPXFHXIUYHLGKPUQEWADHQNOOCQFIALILTYUPOUHLKNOZCNVPZNUXPLHIEVVDYRZBXHMSHKMLMQKQJUIKNDXYIAZROAQGZLFTPTGRKKRJYWJ9IBDSXZGBIXSYQNTOC9KH9TIUELMPUEYPXJHXYLXCGCVHXQWPBQZHLXRVBDWIZYIGVYLDCQLL9TMOGJ9OBMGCWXFVOIHH9JUOCYJGZXXEBAZTRLNFMGNIDRIZTSVHQBCERSLXDEJVGCFEMDQQLOHSVLQNFHKZUKSEJJPQEUVGVBXMLLHJWYGAESAGSPPUQUSWZZUOANONXWVHJOGI9AAIPFVYDLRWOTDEIJPKJMDYPFAXPEEZSQNXBAFRYNFRVIOITDAVIZWGJLRIIDNBLVAHHGR9NDEESNOHWVQUKLFWBSOYJPIXCTBRASSJZRYFYUSRQHREMKWNMJFYIYVRGKFXHBXNLTDXAAGGRHI9JUIFMJTSTMY9CNDQRCXNMFUBXWTMXMDEQEWHMBYXKPKWEBLSKKJCQNEWACPXGAPL9TIFRVOCWVTLCJDDCAEZWMEIJGDGOEIFOBNOIOMEQVMVFPWZSGSNCMRNKTETNSV9OARJIGYQPZVVLUFHICLMQOPAH9XUGEJWUGEXK9JWUQHHFGSGVYHSBIJVUVWVWFEZQNQ9OQNUZZHVLCSASWPBVJ9ADNBKLTBRAMNZHEUSAMMJZDASOKZODKIEKDJAS9SASKIGMHJAVBYQRJOARIBBPHEJIPTMOGXOFFFCRQHFQWBGRVNNNSFFRYJZDIJJBLADPCUVTGRVTTFQRROOSMWKBNJ9PXNKXILUMUKPPIVVTYPI9EZUDNGMRQKCVRACQKAFQDJBFVFVHVULVZKJXKBBYDUAYJXSEWYUBQVFUKETLHCZXIEMM9SJUPVYYIRZOKKXYDJZBWLPUGNWGSKOAOXRYZLFUCSMOLNYFPCCAHZ9GRELEVZPRHWYGHJRAMRHKBIURQPMQEEMTWWTBVZMZKCYGWSESFA9ETEFQGYYYKCQGS9RDAHTG9TVCWWCHJBABMRNNHYHJISANVHNOO9DFTOTYIQVF9YXRFPDSMSCLAGCIZLCDULIMUQPJAFHEDNHCUFMPMLYTWGWNDO9LYIJNGHBM99GNAEWKMTYWBXBDSAIQGDCIXCSKM9KZZGRLRKSQAQCRPTGXPPUTXBAQUUEFDHT9PICRJVPOAVPZKVWJDO9SUACNVBMIQMNHVIAFWXBS9RMNLBZSKOJOHFKQQPVLPPBL9LKZHOVJDEY9AB9JZHSHZCRMKBW9ELSA9

Address:  RAIBGCEYBGRGFDEKZCGTEVJSRJDPRXKRHSUMSYPUTPGIMPSQCJGXSOFIYNY9CBKPBJDNTXAEGFEZ9UZBS

Listen to this stream with

>  node recIT.js RAIBGCEYBGRGFDEKZCGTEVJSRJDPRXKRHSUMSYPUTPGIMPSQCJGXSOFIYNY9CBKPBJDNTXAEGFEZ9UZBS 
```

![](images/Example.png?raw=true)

The message is encrypted

 
 
### Receiving MAM

A channel ID must be provided for listeners to receive and decode MAM messages.  In this example, the sender publishes a CLI run command with the channel ID.  Notice the example above.  Open a new terminal and run that command:

```
node recIT.js RAIBGCEYBGRGFDEKZCGTEVJSRJDPRXKRHSUMSYPUTPGIMPSQCJGXSOFIYNY9CBKPBJDNTXAEGFEZ9UZBS  
```

Step through the receiver script, recIT.js.  First, import the MAM client, mam.client.js and the asciiToTrytes converter.

```
global._babelPolyfill = false
require('./errorhandling')

const Mam = require('../external/mam.client.js')
const { trytesToAscii } = require('@iota/converter')
```


Populate the MAM message:
	mamType – public, private, or restricted.  In this example, public
	mamSecret – used for encryption.  In this example, "DONTSHARETHIS"


```
const mamType = 'public'
const mamSecret = 'DONTSHARETHIS'
```


Keep track of current and next root so the listener knows when and where to retrieve the message

```
let root = null
let nextRoot = process.argv[2]
```


Display message when received

```
function showData (raw) {
  const data = JSON.parse(trytesToAscii(raw))
  console.log(data.ts, '-', data.test)
}
```

Connect to the IRI Node, in this example the Devnet

```
async function initMam () {
  console.log('\r\n\r\n')
  console.log('Listening to MAM stream test...')
  console.log('\r\n')
  await Mam.init('https://nodes.devnet.iota.org:443')
}
```


This listener checks the MAM stream every 5 seconds for new data on the current root.  If a new root is returned, then this new root will be monitored 


```
async function checkMam () {
  if (root !== nextRoot) {
    root = nextRoot
  }

  // The showData callback will be called in order for each message found
  const data = await Mam.fetch(root, mamType, mamSecret, showData)
  nextRoot = data.nextRoot

  // Check again in 5 seconds
  setTimeout(checkMam, 5000)
}
```

The listener starts monitoring 

```
initMam()
checkMam()
```



**Example Output**

```
iotahub@u18:~/HMmam/src$ node recIT.js RAIBGCEYBGRGFDEKZCGTEVJSRJDPRXKRHSUMSYPUTPGIMPSQCJGXSOFIYNY9CBKPBJDNTXAEGFEZ9UZBS

Listening to MAM stream test...

2018-11-10T17:16:23.897Z - MAMTEST
```