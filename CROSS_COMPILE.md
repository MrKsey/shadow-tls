# Кросс-компиляция для MIPS LE Softfloat

## Описание

Этот проект поддерживает компиляцию под архитектуру MIPS Little-Endian с soft-float ABI, что необходимо для работы на роутерах Keenetic и других устройствах с MIPS LE процессором без FPU.

## Используемые файлы

- `.mipsel-softfloat.json` - кастомный target спецификация для Rust с softfloat ABI
- `.github/workflows/keenetic.yml` - GitHub Actions workflow для автоматической сборки

## Локальная сборка

### Требования

1. Rust nightly toolchain:
   ```bash
   rustup install nightly
   rustup default nightly
   ```

2. MIPS кросс-компилятор:
   ```bash
   # Ubuntu/Debian
   sudo apt-get install gcc-mipsel-linux-gnu
   
   # Или используйте prebuilt toolchain
   wget https://musl.cc/mipsel-linux-musl-cross.tgz
   tar xzf mipsel-linux-musl-cross.tgz
   export PATH=$PWD/mipsel-linux-musl-cross/bin:$PATH
   ```

### Команда сборки

```bash
cargo build --release --target ./.mipsel-softfloat.json
```

Бинарник будет создан по пути:
```
target/mipsel-unknown-linux-musl/release/shadow-tls
```

## GitHub Actions

Сборка через GitHub Actions запускается автоматически при публикации релиза или вручную через Actions tab:

1. Перейдите в Actions tab репозитория
2. Выберите "Build Keenetic MIPSLE Softfloat"
3. Нажмите "Run workflow"

## Проверка бинарника

После сборки проверьте архитектуру бинарника:

```bash
file target/mipsel-unknown-linux-musl/release/shadow-tls
```

Ожидаемый вывод должен содержать `MIPS, little endian`.

Для проверки использования soft-float:

```bash
readelf -A target/mipsel-unknown-linux-musl/release/shadow-tls | grep Tag_ABI_Float
```

Должно показать `Tag_ABI_Float: soft`.
