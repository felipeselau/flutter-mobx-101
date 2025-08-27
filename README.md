MobX Flutter Boilerplate — Versão Simples

Um exemplo mínimo para demonstrar MobX no Flutter com apenas main.dart, counter.dart e counter_store.dart.

⸻

🎯 Objetivo
	•	Introduzir MobX sem complicação
	•	Mostrar observables, actions e computed
	•	Tudo em 3 arquivos dentro de lib/

⸻

🧰 Dependências

flutter pub add mobx flutter_mobx
flutter pub add --dev mobx_codegen build_runner

Para gerar o código:

flutter pub run build_runner watch --delete-conflicting-outputs


⸻

📁 Estrutura

lib/
 ├─ main.dart
 ├─ counter.dart
 └─ counter_store.dart


⸻

⚙️ main.dart

import 'package:flutter/material.dart';
import 'counter.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'MobX Counter',
      theme: ThemeData(colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple), useMaterial3: true),
      home: const CounterPage(),
    );
  }
}


⸻

🔢 counter_store.dart

import 'package:mobx/mobx.dart';
part 'counter_store.g.dart';

class CounterStore = _CounterStore with _$CounterStore;

abstract class _CounterStore with Store {
  @observable
  int value = 0;

  @computed
  bool get isEven => value % 2 == 0;

  @action
  void increment() => value++;

  @action
  Future<void> loadInitial() async {
    await Future.delayed(const Duration(milliseconds: 300));
    value = 42;
  }
}


⸻

🖥️ counter.dart

import 'package:flutter/material.dart';
import 'package:flutter_mobx/flutter_mobx.dart';
import 'counter_store.dart';

class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  final store = CounterStore();

  @override
  void initState() {
    super.initState();
    store.loadInitial();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('MobX Counter')),
      body: Center(
        child: Observer(
          builder: (_) => Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              Text('Valor: ${store.value}', style: const TextStyle(fontSize: 28)),
              const SizedBox(height: 8),
              Text(store.isEven ? 'Par' : 'Ímpar'),
              const SizedBox(height: 16),
              ElevatedButton(
                onPressed: store.loadInitial,
                child: const Text('Carregar inicial (42)'),
              ),
            ],
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: store.increment,
        child: const Icon(Icons.add),
      ),
    );
  }
}


⸻

✅ Checklist
	•	Rodar build_runner para gerar counter_store.g.dart
	•	Testar increment e computed