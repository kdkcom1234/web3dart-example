Binance Smart Chain (BSC)은 Ethereum Virtual Machine (EVM)과 호환되므로, Ethereum 기반의 라이브러리인 `web3dart`를 사용해 BSC에서 트랜잭션을 전송할 수 있습니다.

다음은 BSC 테스트넷에서 다른 지갑 주소로 BNB (BSC의 기본 토큰)를 전송하는 예제입니다:

1. 먼저, 필요한 패키지를 `pubspec.yaml`에 추가해야 합니다:

```yaml
dependencies:
  web3dart: ^latest_version
  http: ^latest_version
```

2. 이제 트랜잭션을 전송하는 예제를 작성합니다:

```dart
import 'dart:math';
import 'package:http/http.dart';
import 'package:web3dart/web3dart.dart';

void sendTransaction(String privateKey, String toAddress, BigInt amount) async {
  // BSC 테스트넷 RPC URL (해당 URL은 바뀔 수 있으므로, 항상 최신 정보를 확인하세요)
  final rpcUrl = 'https://data-seed-prebsc-1-s1.binance.org:8545/';
  final client = Web3Client(rpcUrl, Client());

  final credentials = await client.credentialsFromPrivateKey(privateKey);
  final senderAddress = await credentials.extractAddress();

  // 트랜잭션 정보 구성
  final transaction = Transaction(
    to: EthereumAddress.fromHex(toAddress),
    gasPrice: EtherAmount.inWei(BigInt.one),
    maxGas: 100000,
    value: EtherAmount.inWei(amount),
    nonce: await client.getTransactionCount(senderAddress),
  );

  final response = await client.sendTransaction(
    credentials,
    transaction,
    chainId: 97, // BSC 테스트넷의 Chain ID
  );

  print('Transaction hash: ${response}');

  // 트랜잭션 영수증 검색 (옵션)
  final receipt = await client.getTransactionReceipt(response);
  if (receipt == null) {
    print('Transaction pending');
  } else if (receipt.status) {
    print('Transaction successful');
  } else {
    print('Transaction failed');
  }

  client.dispose();
}

void main() {
  final myPrivateKey = 'YOUR_PRIVATE_KEY';
  final destinationAddress = 'DESTINATION_WALLET_ADDRESS';
  final sendAmount = BigInt.from(pow(10, 18) * 0.01); // 예: 0.01 BNB

  sendTransaction(myPrivateKey, destinationAddress, sendAmount);
}
```

주의:

- 개인 키를 직접 코드에 입력하는 것은 권장되지 않습니다. 항상 보안에 신경 써서 개인 키를 관리하세요.
- 이 코드는 간단한 예시로만 제공됩니다. 실제 생산 환경에서는 추가적인 에러 처리, 로깅, 기타 최적화 작업이 필요합니다.
