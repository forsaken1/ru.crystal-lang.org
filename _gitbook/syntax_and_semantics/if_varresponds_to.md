# if var.responds_to?(...)

Если в условии `if` есть проверка `responds_to?`, то в ветке `then` тип переменной гарантированно будет ограничен теми типами, на которые отвечает этот метод:

```crystal
if a.responds_to?(:abs)
  # здесь тип a будет ограничен теми, на которые отвечает метод 'abs'
end
```

Кроме того, в ветке `else` тип переменной гарантированно будет ограничен теми типами, на которые метод не отвечает:

```crystal
a = some_condition ? 1 : "hello"
# a : Int32 | String

if a.responds_to?(:abs)
  # здесь a будет Int32, поскольку Int32#abs существует, а String#abs нет
else
  # здесь a будет String
end
```

Сказанное выше **не работает** с переменными объекта, класса и глобальными переменными. Для работы с ними присвойте их значение локальной переменной:

```crystal
if @a.responds_to?(:abs)
  # нельзя утверждать, что @a ответит на `abs`
end

a = @a
if a.responds_to?(:abs)
  # здесь a гарантированно отвечает на `abs`
end

# Немного короче:
if (a = @a).responds_to?(:abs)
  # здесь a гарантированно отвечает на `abs`
end
```
