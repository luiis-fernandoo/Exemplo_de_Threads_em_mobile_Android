# Threads: Visão Geral dos Processos e Threads no Android
## Introdução
Quando um componente de aplicativo é iniciado no sistema Android e não há outro componente em execução, o sistema cria um novo processo no Linux para o aplicativo, equipado com um único thread de execução. Este thread é chamado de "principal" e, por padrão, todos os componentes do mesmo aplicativo são executados no mesmo processo e thread. Este README explora a dinâmica de processos e threads em aplicativos Android.

## Processos
Por padrão, todos os componentes de um aplicativo são executados no mesmo processo. No entanto, é possível personalizar o processo associado a um componente no manifesto do aplicativo usando o atributo android:process. Isso permite que alguns componentes compartilhem um processo enquanto outros podem ter seus próprios processos. O Android pode desativar um processo quando a memória está baixa, reiniciando-o quando necessário.

## Manifesto
O arquivo de manifesto do aplicativo desempenha um papel crucial no controle do processo relacionado a um componente. Cada tipo de elemento de componente (activity, service, receiver, provider) pode ter o atributo android:process definido para especificar o processo em que deve ser executado.

## Ciclo de Vida do Processo
O ciclo de vida do processo está intrinsecamente ligado aos estados do aplicativo, discutidos em detalhes no documento sobre o ciclo de vida dos processos e do aplicativo.

## Threads
Quando um aplicativo é executado, o sistema cria um thread de execução chamado "principal". Esse thread é responsável por despachar eventos para a interface do usuário, tornando-o crucial para a interação do usuário. No entanto, em circunstâncias especiais, o thread principal pode não ser o thread de IU.

## Thread Principal e IU
O thread principal, também conhecido como thread de IU, é onde ocorre a interação com componentes da IU do Android. Operações intensivas no thread principal podem resultar em desempenho inferior e até ANRs (Application Not Responding). O sistema Android não cria um thread separado para cada instância de componente; todos os componentes em um processo compartilham o mesmo thread de IU.

## Threads de Trabalho
Para evitar bloqueios no thread principal, especialmente durante operações demoradas como acesso à rede ou consultas a banco de dados, é vital usar threads de trabalho. No entanto, é crucial lembrar que a IU não pode ser atualizada a partir de threads diferentes do thread de IU.

## Estratégias para Threads de Trabalho
O Android oferece métodos para acessar o thread de IU a partir de threads de trabalho, como Activity.runOnUiThread(Runnable) e View.post(Runnable).

```
fun onClick(v: View) {
    Thread(Runnable {
        // tarefa potencialmente demorada
        val bitmap = processBitMap("image.png")
        imageView.post {
            imageView.setImageBitmap(bitmap)
        }
    }).start()
}
```

## Refêrencia
Developers Android: https://developer.android.com/guide/components/processes-and-threads?hl=pt-br
