<template>
	<div class="py-16 xl:divide-y xl:divide-gray-200">
		<header class="pt-6 xl:pb-10">
			<div class="space-y-1 text-center">
				<div>
					<h1 class="text-3xl leading-9 font-extrabold text-gray-900 tracking-tight sm:text-4xl sm:leading-10 md:text-5xl md:leading-14">
						Configuration de Laravel Sanctum pour l'authentification SPA avec Vue.js
					</h1>
				</div>
			</div>
		</header>
		<div>
			<div class="relative py-16 overflow-hidden">
				<div class="relative px-4 sm:px-6 lg:px-8">
					<div class="text-lg max-w-prose mx-auto">
						<p class="mt-8 text-xl text-gray-500 leading-8">
							Laravel Sanctum (anciennement connu sous le nom de Laravel Airlock) est un package Laravel officiel permettant de gérer à la fois le jeton API et l'authentification SPA (Single Page Application).
							Nous nous concentrons sur l'authentification SPA à l'aide d'une simple application Vue.js. Il est vraiment important de noter que ce guide n'a rien à voir avec l'émission et l'utilisation de jetons pour communiquer avec une API.
						</p>
					</div>
					<div class="mt-8 prose prose-blue prose-lg text-gray-500 mx-auto">
						<h2>Pourquoi Sanctum ?</h2>
						<p>
							Si vous authentifiez déjà des utilisateurs sur votre SPA, vous utilisez probablement JWT (jetons Web JSON). J'ai toujours opté pour l'authentification JWT, mais avec l'arrivée de Sanctum, je l'utilise désormais exclusivement lorsque je mets au point un nouveau projet. Parce que c'est génial.
						</p>
						<ul>
							<li>C'est un package Laravel officiel, vous êtes donc à peu près assuré qu'il s'intégrera sans effort au framework.</li>
							<li>Vous n'avez pas à vous soucier du stockage manuel des jetons sur le client, car l'authentification est gérée par le biais de cookies.</li>
						</ul>
						<h2>Comment fonctionne Sanctum ?</h2>
						<p>Avant de commencer à écraser aveuglément sans comprendre ce qui se passe dans les coulisses, passons en revue le fonctionnement de Sanctum.</p>
						<p>Sanctum utilise l'authentification de session basée sur les cookies de Laravel pour authentifier les utilisateurs de votre client. Voici le flux :</p>
						<ul>
							<li>Vous demandez un cookie CSRF à Sanctum sur le client, ce qui vous permet de faire des requêtes protégées par CSRF à des points de endpoint normaux tels que /login .</li>
							<li>Vous faites une demande au point de endpoint Laravel /login normal.</li>
							<li>Laravel émet un cookie contenant la session de l'utilisateur.</li>
							<li>Toutes les demandes adressées à votre API incluent désormais ce cookie, de sorte que votre utilisateur est authentifié pour la durée de vie de cette session.</li>
						</ul>
						<p>
							Et vous n'avez pas besoin de faire grand-chose pour que cela fonctionne. Le plus dur est l'installation et la configuration de Sanctum.
						</p>


						<h2>Installation et configuration de Sanctum</h2>
						<p>
							Nous allons commencer par une nouvelle installation de Laravel 8 pour ne pas compliquer les choses. Si vous avez déjà un projet vers lequel vous basculez (en particulier si vous utilisez une ancienne version de Laravel), il peut y avoir des différences ici. Ma recommandation serait de configurer cela avec un nouveau projet Laravel 8 d' abord pour comprendre Sanctum, puis de définir les détails de votre projet actuel.
						</p>
						<p>Allons-y.</p>

						<h2>Un nouveau projet Laravel</h2>
						<p>
							Vous savez probablement déjà comment faire cela, mais créons un nouveau projet Laravel, que j'ai appelé, avec imagination, testingsanctum.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							composer create-project laravel/laravel testingsanctum
						</div>
						<p>
							Au lieu d'utiliser Homestead, Valet ou Docker, gardons les choses simples et servons Laravel localement.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							php artisan serve
						</div>
						<p>
							N'utilisez pas 127.0.0.1, utilisez plutôt localhost . Ceci est vraiment important plus tard lorsque nous configurerons nos domaines Sanctum et nos origines CORS.
						</p>
						<p>
							Enfin, assurez-vous de changer les paramètres de connexion de votre base de données afin que nous puissions utiliser une base de données. J'utilise Postgres localement, alors voici le mien:
						</p>
						<figure>
							<img class="w-full rounded-lg" src="~assets/images/db_config.png" alt="" width="1310" height="873">
						</figure>

						<h2>Authentification avec laravel / ui</h2>
						<p>
							Oui, nous avons toujours besoin de nos contrôleurs d'authentification standard, et avec Laravel 8, ceux-ci sont cachés dans le paquet laravel / ui .
						</p>
						<p>
							Installez d' abord le paquet laravel / ui .
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							composer require laravel/ui
						</div>
						<p>
							Ensuite, générez l'échafaudage d'authentification (nous utilisons simplement Bootstrap ici).
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							php artisan ui bootstrap --auth
						</div>
						<p>
							Et enfin compilez les assets, nous avons donc un style sur nos pages d'authentification.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							npm install && npm run dev
						</div>
						<p>
							Nous n'en avons pas vraiment besoin, mais cela aide si vous souhaitez toujours utiliser l'authentification Web standard pour votre projet et utiliser des composants Vue dans Laravel qui effectuent des demandes de endpoints authentifiés.
						</p>

						<h2>Installez Laravel Sanctum</h2>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							compoer require laravel/sanctum
						</div>
						<p>
							Publiez maintenant les fichiers de configuration et les migrations.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
						</div>
						<p>
							N'hésitez pas à supprimer la migration créée si vous ne souhaitez pas ajouter la table à votre base de données. J'aime le garder là de toute façon, juste au cas où je voudrais utiliser cette fonctionnalité de Sanctum plus tard.
						</p>
						<p>
							Exécutez vos migrations
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							php artisan migrate
						</div>
						<p>
							Important, car cela crée la table des utilisateurs , dont nous avons besoin pour l'authentification.
						</p>
						<p>
							Ajoutez maintenant le <strong>EnsureFrontendRequestsAreStatefulmiddleware à votre apigroupe de middleware, dans app/Http/Kernel.php</strong>.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							use Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful; <br />

							'api' => [ <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EnsureFrontendRequestsAreStateful::class, <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'throttle:60,1', <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\Illuminate\Routing\Middleware\SubstituteBindings::class, <br />
							],
						</div>
						<p>
							Cela garantit que les demandes adressées à notre API peuvent utiliser des cookies de session, car c'est ainsi que Sanctum s'authentifie lors des demandes.
						</p>


						<h2>Configurer Sanctum</h2>
						<p>
							Ouvrez le fichier <strong>config/sanctum.php</strong> et jetez un œil. Il est essentiel que nous définissions la statefulclé pour qu'elle contienne une liste de domaines dont nous acceptons les demandes authentifiées.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							'stateful' => explode(',', env('SANCTUM_STATEFUL_DOMAINS', 'localhost,127.0.0.1')),
						</div>
						<p>
							Heureusement pour nous, localhost est déjà là, donc nous sommes prêts à partir. Vous devrez changer cela lors du déploiement en production, donc ajouter <strong>SANCTUM_STATEFUL_DOMAINS</strong> à votre fichier <strong>.env</strong> une liste de domaines autorisés séparés par des virgules est une excellente idée.
						</p>
						<p>
							J'ai tendance à ajouter cela de toute façon, car cela définit mon fichier <strong>.env</strong> avec une référence à ce que je devrais changer en production.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							SANCTUM_STATEFUL_DOMAINS=localhost
						</div>

						<h2>
							Changer le pilote de session
						</h2>
						<p>
							Dans <strong>.env</strong>, mettez à jour votre pilote de session pour utiliser autre chose que file. L' cookieoption fonctionnera bien pour le moment.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							SESSION_DRIVER=cookie
						</div>

						<h2>Configurer CORS</h2>
						<p>
							Laravel 8 est livré avec le package fruitcake / laravel-cors . Accédez à votre fichier de configuration <strong>config/cors.php</strong> et mettez paths à jour le pour qu'il ressemble à ceci:
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							'paths' => [ <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'api/*', <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'/login', <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'/logout', <br />
							&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'/sanctum/csrf-cookie' <br />
							]
						</div>
						<p>
							Parce que nous allons potentiellement faire des requêtes à partir d'un autre domaine, nous nous assurons qu'en plus de nos endpoints d'API, nous autorisons également les requêtes cross-origin à / login et / logout, ainsi que le spécial / sanctum / csrf-cookie endpoint (plus d'informations à ce sujet plus tard).
						</p>
						<p>
							Vous voudrez également définir l'option <strong>supports_credentials</strong> sur <strong>true</strong>.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							'supports_credentials' => true
						</div>

						<h2>Middleware Sanctum</h2>
						<p>
							Pour le moment, dans <strong>routes/api.php</strong>, nous avons le <strong>auth:apimiddleware</strong> défini pour l'exemple de route d'API fourni par Laravel. Cela ne fonctionnera pas, car lorsque nous enverrons finalement une demande de notre client, nous aurons besoin de Sanctum pour récupérer le cookie de session et déterminer si nous sommes authentifiés ou non.
						</p>
						<p>
							Mettez à jour cette route pour utiliser le <strong>auth:sanctummiddleware</strong> à la place.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							Route::middleware('auth:sanctum')->get('/user', function (Request $request) { <br /> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return $request->user(); <br /> });
						</div>
						<p>
							Nous toucherons ce point de terminaison plus tard pour vérifier si notre processus d'authentification a fonctionné.
						</p>


						<h2>
							Créer un utilisateur avec lequel tester
						</h2>
						<p>
							Faites-le via l'interface utilisateur Web ou sur la ligne de commande avec Tinker, selon votre préférence. Si vous êtes curieux, voici la commande pour créer un utilisateur avec Tinker.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							php artisan tinker
						</div>
						<div class="mt-2 rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							factory(App\User::class)->create(['name' => 'SIDIBE Cheick Ahmed', 'email' => 'sidibe.cheickahmed@eemi.com', 'password' => bcrypt('ilovecats')]);
						</div>
						<p>
							Et avec tout cela fait, nous sommes presque prêts à partir. Récapitulons d'abord.
						</p>

						<h2>
							Recapitulatif
						</h2>
						<ul>
							<li>Nous avons installé Laravel, évidemment.</li>
							<li>Nous avons nos contrôleurs d'authentification normaux extraits de laravel / ui , car nous allons leur faire des requêtes de la part de notre client.</li>
							<li>Sanctum est installé, configuré et nous autorisons les requêtes authentifiées de localhost.</li>
							<li>Nous pouvons faire des requêtes cross-origin à / login, / logout et / sanctum / csrf-cookie.</li>
						</ul>


						<h2>
							Test
						</h2>
						<p>
							La meilleure façon de vérifier que tout fonctionne est de créer un projet Vue CLI complètement nouveau et de faire quelques demandes. Encore une fois, vous avez peut-être déjà un SPA prêt à être déployé, mais testez-le avec un tout nouveau et déterminez les spécificités de votre implémentation actuelle plus tard.
						</p>

						<h2>
							Installez Vue CLI et créez un projet
						</h2>
						<p>
							Si vous n'avez pas déjà installé Vue CLI, c'est simple:
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							npm install -g @vue/cli
						</div>
						<p>
							Créez ensuite un nouveau projet.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							vue create testingsanctumclient
						</div>
						<p>
							Vous trouverez ci-dessous les options que vous voudrez sélectionner. Je tire sur Vuex parce que nous voulons un bon moyen de garder une trace de l'état de notre utilisateur authentifié. Nous aurons également besoin de Vue Router pour créer une page de connexion distincte.
						</p>
						<p>
							Pour toutes les invites ultérieures que Vue CLI vous lance, choisissez les options que vous préférez.
						</p>
						<p>
							Une fois installé, accédez au répertoire du projet Vue et exécutez la commande <strong>npm run serve</strong> pour que votre client soit opérationnel.
						</p>

						<h2>Créer une page de connexion</h2>
						<p>
							Créez un nouveau fichier, <strong>views/login.vue</strong> avec un simple formulaire de connexion.
						</p>

						<figure>
							<img class="w-full rounded-lg" src="~assets/images/loginscreen.png" alt="" width="1310" height="873">
						</figure>

						<figure>
							<img class="w-full rounded-lg" src="~assets/images/loginCode.png" alt="" width="1310" height="873">
						</figure>


						<h2>
							Ajouter un état Vuex
						</h2>
						<p>
							Je préfixerai ceci avec, pourquoi utilisons-nous Vuex? Eh bien, puisque nous voulons conserver un `` état '' global authentifié dans notre client, l'utilisation d'une bibliothèque de gestion d'état comme Vuex a du sens ici. Cela nous permettra également de vérifier facilement dans n'importe quel composant si nous sommes authentifiés ou non (par exemple, notre navigation).
						</p>
						<p>
							Commencez par créer un fichier <strong>store/auth.js</strong> avec les éléments suivants.
						</p>
						<figure>
							<img class="w-full rounded-lg" src="~assets/images/StoreCode.png" alt="" width="1310" height="873">
						</figure>

						<p>
							Si vous avez déjà utilisé Vuex, cela devrait être assez facile à comprendre. Sinon, nous venons de créer un module Vuex spécifiquement pour la fonctionnalité d'authentification, il est donc parfaitement rangé.
						</p>
						<p>
							La propriété <strong>state</strong> détient si nous sommes authentifiés ou non, et contient les détails de l'utilisateur que nous allons récupérer une fois authentifié.
						</p>
						<p>
							Notre <strong>mutations</strong> met à jour notre state. Par exemple, une fois que nous sommes authentifiés avec succès, nous commettrons une mutation pour définir authentifié trueet commettons une autre mutation pour définir les détails de l'utilisateur.
						</p>
						<p>Ajoutez maintenant le module auth à Vuex dans <strong>store/index.js</strong>.</p>
						<figure>
							<img class="w-full rounded-lg" src="~assets/images/2.png" alt="" width="1310" height="873">
						</figure>

						<h2>
							Ajouter quelques actions
						</h2>
						<p>
							Mettons à jour le fichier <strong>store/auth.js</strong> et ajoutons quelques actions. N'oubliez pas d'importer axios en haut !
						</p>
						<figure>
							<img class="w-full rounded-lg" src="~assets/images/3.png" alt="" width="1310" height="873">
						</figure>
						<p>
							L'action <strong>signIn</strong> fait d'abord une requête à / sanctum / csrf-cookie. Nous l'avons ajouté à nos chemins CORS plus tôt, vous vous souvenez? Faire une demande GET à ce point de terminaison demande à notre client de définir un cookie CSRF, de sorte que les demandes supplémentaires adressées à notre API incluent un jeton CSRF. Ceci est vraiment important, car nous ne voulons pas désactiver les vérifications CSRF sur les points de terminaison normaux tels que / login. C'est ça.
						</p>
						<p>
							Suite à cela, l'action <strong>signIn</strong> effectue une demande normale de /connexion avec les informations d'identification que nous fournissons.
						</p>
						<p>
							À ce stade de l'envoi de cette action, nous devrions être totalement authentifiés. En envoyant une requpete ) /login comme d'habitude, notre client définira un cookie de session authentifié pur cet uutilisateur. Cela signifie que toutes les demandes supplémentaires adressées aux points de terminaison d'API authentifiés devraient fonctionner. La magie!
						</p>
						<p>
							Une fois que tout cela est fait, nous envoyons l' meaction, qui fait une requête à la route / api / user. Maintenant que nous avons ce cookie de session, cela devrait nous renvoyer avec succès les détails de notre utilisateur authentifié, que nous définissons dans notre état, avec un indicateur nous indiquant que nous sommes authentifiés.
						</p>
						<p>
							Enfin, si nous distribuons l'action <strong>me</strong> et que cela échoue, nous revenons à un état non authentifié.
						</p>

						<h2>Ajuster axios</h2>
						<p>
							Avant de faire ces demandes, nous devons définir une URL de base pour notre API (notez que celles-ci ne sont pas incluses dans les demandes que nous avons actuellement) et activer également l'option <strong>withCredentials</strong>.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							axios.defaults.withCredentials = true <br />
							axios.defaults.baseURL = 'http://localhost:8000/'
						</div>
						<p>
							Psst, puisque nous définissons les valeurs par défaut pour axios, vous devrez vous assurer de l'importer en haut de <strong>main.js</strong>.
						</p>
						<div class="rounded-md bg-gray-800 p-4 overflow-x-auto text-gray-300 tracking-wide">
							import axios from 'axios'
						</div>
						<p>
							L'option <strong>withCredentials</strong> est vraiment importante ici. Cela demande à axios d'envoyer automatiquement notre cookie d'authentification avec chaque demande.
						</p>


						<h2>
							Connexion
						</h2>
						<p>
							J'espère que vous comprendrez ce qui se passe dans notre boutique Vuex. Connectons cela afin que nous puissions entrer des informations d'identification et nous authentifier.
							Mettez à jour <strong>views/SignIn.vue</strong> avec les éléments suivants.
						</p>
						<figure>
							<img class="w-full rounded-lg" src="~assets/images/4.png" alt="" width="1310" height="873">
						</figure>
						<p>
							Cela relie nos champs de courrier électronique et de mot de passe aux données du composant, puis ajoute un gestionnaire d'événements d'envoi au formulaire global. Une fois appelé, notre gestionnaire de formulaire distribue l'action <strong>signIn</strong> depuis notre magasin et fait tout ce dont nous avons parlé dans la dernière section.
						</p>
						<p>
							Vous devriez maintenant pouvoir vous diriger vers la page / signin et l'essayer avec l'utilisateur que vous avez créé précédemment. Essayer!
						</p>
						<p>
							Si tout se passe bien, vous devriez voir les choses suivantes.
						</p>
						<p>
							Le cookie laravel_session de Laravel et le cookie XSRF-TOKEN .
						</p>
						<p>
							Votre état Vuex a été mis à jour pour refléter le fait que nous sommes connectés, ainsi que les détails de l'utilisateur (vous devrez peut-être cliquer sur `` charger l'état '' dans Vue devtools pour voir cela).
						</p>

						<!-- END -->
					</div>
				</div>
			</div>

		</div>
	</div>
</template>

<script>
export default {
	head () {
		return {
			title : 'Tuto SIDIBE CHEICK AHMED'
		}
	}
}
</script>