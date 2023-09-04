바이낸스 스마트체인 (BSC)은 Ethereum Virtual Machine (EVM)과 호환되기 때문에 Ethereum과 동일한 방식으로 BSC의 지갑 잔액을 조회할 수 있습니다. `web3dart` 라이브러리를 사용하면 이를 쉽게 할 수 있습니다.

다음은 BSC 테스트넷에서 지갑의 BNB 잔액을 조회하는 예제입니다:

1. 필요한 패키지를 `pubspec.yaml`에 추가합니다:

```yaml
dependencies:
  web3dart: ^latest_version
  http: ^latest_version
```

2. 잔액 조회를 위한 함수와 예제 호출을 작성합니다:

```dart
import 'package:http/http.dart';
import 'package:web3dart/web3dart.dart';

Future<EtherAmount> getBalance(String walletAddress) async {
  // BSC 테스트넷 RPC URL (이 URL은 변경될 수 있으므로, 항상 최신 정보를 확인하세요)
  final rpcUrl = 'https://data-seed-prebsc-1-s1.binance.org:8545/';
  final client = Web3Client(rpcUrl, Client());

  final address = EthereumAddress.fromHex(walletAddress);
  final balance = await client.getBalance(address);

  client.dispose();

  return balance;
}

void main() async {
  final address = 'YOUR_WALLET_ADDRESS';
  final balance = await getBalance(address);

  // Wei에서 Ether로 변환 (1 Ether = 10^18 Wei)
  print('Balance of $address: ${balance.getInEther} BNB');
}
```

주의:

- 이 코드는 간단한 예시를 제공하기 위해 작성되었습니다. 실제 사용할 때는 에러 처리, 로깅 및 기타 최적화 작업을 고려해야 합니다.
- BSC RPC URL은 변경될 수 있으므로, 바이낸스의 공식 문서나 다른 신뢰할 수 있는 출처에서 최신 URL 정보를 확인하세요.
