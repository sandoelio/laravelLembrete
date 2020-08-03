# Laravel

## 1º Instalação do composer
baixar o composer e instalar para verificar usar o comando 'composer'.
<br>
## 2º Instalação do Laravel e projeto em pasta especifica
Este comando do composer vai baixar o laravel em um diretório (pasta) chamada “name-project” '
composer create-project --prefer-dist laravel/laravel name-project'
<br>
## 3º Cria o banco de dados sem as tabelas.
<br>

## 4º Configurações do banco mysql
no arquivo.env,precisa configurar nome da aplicação e URL :
APP_NAME=EspecializaTi_Curso_Laravel 
APP_URL=http://dev.laravel 
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=name_database
DB_USERNAME=root
DB_PASSWORD=
<br>
##5º Gerar migrates
php artisan migrate (caso as tabelas ja tenha sido criadas)
devem começar com create e terminar com table (Exemplo:create_password_resets_table)
'php artisan make:migration create_products_table'
						OU
opção menos -m indica que é para criar o Model e também o arquivo de migration
'php artisan make:model Product –m'
<br>
##6º Definir dados no migrates
Schema::create('nome_tabela_aqui', function (Blueprint $table) { 
	$table->increments('id');// Id da tabela (chave primária e incremento)
    $table->integer('category_id')->unsigned();// Id da tabela categories
    $table->foreign('category_id')->references('id')->on('categories'); // Define o campo como chave extrangeira (foreign key)
    $table->string('name', 100)->unique(); // Nome do Produto
    $table->string('image', 100)->nullable(); // Imagem do Produto
    $table->text('description')->nullable(); // Descrição do produto
    $table->timestamps();
});
<br>

##7º Gerar atualizaçoes da tabela
gerar (criar) as tabelas : php artisan migrate
para deletar : php artisan migrate:rollback ou php artisan migrate:reset
para atualizar: php artisan migrate:refresh
<br>
##8º Controller
Para criar um controller e seu CRUD
'php artisan make:controller CategoryController --resource'

index() – Para listar as categorias
create() – Para exibir o formulário de inserção de um nova categoria
store() – Para salvar a categoria (inserir no banco de dados)
show() – Para exibir os detalhes da categoria
edit() – Exibir o formulário para editar a categoria
update() – Para atualizar os dados da categoria
destroy() – Para excluir a(s) categoria(s)
<br>
##9º Rotas
$this->get($uri, $callback);
$this->post($uri, $callback);
$this->put($uri,'NomeController@nomeMetodo');
$this->patch($uri,'NomeController@nomeMetodo');
$this->delete($uri,'NomeController@nomeMetodo');
$this->options($uri,'NomeController@nomeMetodo');
			OU
Route::resource('url','NomeController@nomeMetodo');->funciona se o controller feito com resource
Route::get('url','NomeController@nomeMetodo');->retornar algo, como por exemplo uma listagem
Route::post('url','NomeController@nomeMetodo');->cadastrar algo no sistema
Route::put('url','NomeController@nomeMetodo');->editar algum registro.
Route::delete('url','NomeController@nomeMetodo');->para deletar algo.
Route::options('url','NomeController@nomeMetodo');
Route::any('url','NomeController@nomeMetodo');->aceita qualquer requisição, não importa se é GET, POST, PUT, PATH ou DELETE
###Rotas Nomeadas
Route::get('contact', 'SiteControler@contact')->name('contact');
Route::get('post/{id}', 'PostControler@show')->name('post.show');->com parametro
###Grupo de Rotas:
//admin
Route::group(['prefix' => 'admin'], function(){
    Route::get('users', 'UserController@index');
    Route::get('financeiro', 'FinanceiroController@index');
    Route::get('/', 'AdminController@index');
    //Equivale ao acessar: http://sua-app.dev/admin
});
###Middleware:
permitir acesso a apenas usuários autenticados
Route::get('financeiro','FinanceiroController@index')->middleware('auth');
<br>

#Lembretes
<br>

##Usando os metodos do controller para cadastro de user
método store() e index()

'Construct'
private $usuario;
public function __construct(User $usuario){
     $this->usuario = $usuario;
}
public function index()
    {
        $title = 'Titulo teste';
        return view('painel.form', compact('title'));    
    }
public function store(Request $request)
{
    $dataForm = $request->all(); ->recebe todos os dados do form
    $insert = $this->usuario->create($dataForm); ->insere
    if ($insert) 
        return redirect()->route('cadastro.index');->cadastro e o nome na url e index o controller
    else
        return redirect()->route('cadastro.create');   
}
<br>


##coloca no formulario seguranca
{!! csrf_field() !!} -> O mesmo vale para as rotas POST, PUT e DELETE

Rotas PUT, PATH ou DELETE:
Para enviar requisições para uma destas rotas precisa especificar o método de envio na requisição

<form method="POST" action="/usuario/12">
    <!--Precisa enviar o tokem-->
    {!! csrf_field() !!}
    <!--Espeficica o tipo de envio (verbo http)-->
    <input type="hidden" name="_method" value="PUT">
    <!-- Ou -->
    {{ method_field('PUT') }}
</form>
<br>

## Login sem admiinlte

composer require laravel/ui --dev (NO LARAVEL 6 SUBSTITUI O ARTISAN MAKE:AUTH)
php artisan ui vue --auth
php artisan migrate

## Gerar chave 

php artisan key:generate

## gerar crud no infyom

php artisan infyom:scaffold aluno --fromTable --tableName=aluno

## para dar o refrech nas tabelas 

$table->softDeletes();

## CONFIGURAR EMAIL GMAIL
   config/mail.php
   'host' => env('MAIL_HOST', 'smtp.gmail.com'),
   'from' => [
        'address' => env('MAIL_FROM_ADDRESS', 'camuelsantos@gmail.com'),
        'name' => env('MAIL_FROM_NAME', 'vegefoods'),
    ],

    rota: 
    Route::post('/envio','EmailController@store')->name('mail.store');

    controle:

    use Illuminate\Support\Facades\Mail;
    use Illuminate\Support\Facades\Redirect;

    class EmailController extends Controller
{
    public function store(Request $request)
    {

        Mail::send('contatoEmail', $request->all(), function ($msg) {
            $msg->subject('Vegefoods Pedido');
            $msg->to('camuelsantos@gmail.com');
        });
       return Redirect::to('/');

    }
}

env:

MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=camuelsantos@gmail.com
MAIL_PASSWORD=etujeeaypglrkwqb
MAIL_ENCRYPTION=tls

form so funciona com esse :{!! Form:: open(['route'=>'mail.store', 'method' => 'POST']) !!}{!!Form:: close()!!}

## Chamando um arquivo JS no laravel
   <script type="text/javascript" src="<?php echo asset('public/js/funcao.js')?>"></script>
## Comando para criar Model/Controller/Rotas ao mesmo tempo
   php artisan make:model Product --mcr
## Comando para dropar dados e criar novamente as migrate
   php artisan migrate:fresh
