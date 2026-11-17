
import 'package:flutter/material.dart';

void main() {
  runApp(NutriAfricaApp());
}

// ==================== DONNÉES ====================
class Plat {
  String nom;
  String description;
  int calories;
  int prix;
  String categorie;
  Plat(this.nom, this.description, this.calories, this.prix, this.categorie);
}

class ProduitVivrier {
  String nom;
  String unite;
  int prix;
  String vendeur;
  String imageEmoji;
  ProduitVivrier(this.nom, this.unite, this.prix, this.vendeur, this.imageEmoji);
}

List<Plat> plats = [
  Plat('Poulet grillé + Légumes', 'Pour diabétique, sans sucre', 350, 5000, 'Diabétique'),
  Plat('Poisson braisé + Plantain', 'Riche en protéines', 420, 6000, 'Sportif'),
  Plat('Salade Avocat + Oeuf', 'Pour perte de poids', 280, 4500, 'Minceur'),
  Plat('Ekwang Light', 'Sans trop d\'huile, version saine', 380, 4000, 'Minceur'),
  Plat('Ndolè Light', 'Moins d\'arachide, plus de légumes', 450, 5500, 'Diabétique'),
];

List<ProduitVivrier> vivriers = [
  ProduitVivrier('Manioc frais', '1 tas / 5kg', 2500, 'Mama Ngo - Odza', '🍠'),
  ProduitVivrier('Plantain mûr', '1 régime', 4000, 'Ferme Biyem-Assi', '🍌'),
  ProduitVivrier('Légumes Ndolè', '1 botte', 1000, 'Marché Mokolo', '🥬'),
  ProduitVivrier('Riz local', 'Sac 5kg', 6500, 'Coopérative Nord', '🍚'),
  ProduitVivrier('Huile de palme', 'Bidon 5L', 8000, 'Ferme Douala', '🫙'),
  ProduitVivrier('Haricot rouge', 'Seau 5kg', 7000, 'Ferme Bafoussam', '🫘'),
  ProduitVivrier('Igname', '1 tubercule', 3500, 'Mama Ngo - Odza', '🍠'),
  ProduitVivrier('Tomates fraîches', '1 cageot', 3000, 'Marché Mokolo', '🍅'),
  ProduitVivrier('Oeufs frais', 'Plaquette 30', 3000, 'Ferme Yaoundé', '🥚'),
  ProduitVivrier('Miel pur', 'Pot 1L', 10000, 'Coopérative Ouest', '🍯'),
];

List<Map<String, dynamic>> panier = [];

// ==================== APP PRINCIPALE ====================
class NutriAfricaApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'NUTRI-AFRICA',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(primarySwatch: Colors.green, useMaterial3: true),
      home: PagePrincipale(),
    );
  }
}

class PagePrincipale extends StatefulWidget {
  @override
  _PagePrincipaleState createState() => _PagePrincipaleState();
}

class _PagePrincipaleState extends State<PagePrincipale> {
  int _index = 0;
  final pages = [AccueilPage(), MarcheVivrierPage(), ProfilSantePage(), PanierPage()];
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: pages[_index],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: _index,
        onTap: (i) => setState(() => _index = i),
        type: BottomNavigationBarType.fixed,
        selectedItemColor: Colors.green,
        items: [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Accueil'),
          BottomNavigationBarItem(icon: Icon(Icons.store), label: 'Marché Vivrier'),
          BottomNavigationBarItem(icon: Icon(Icons.monitor_weight), label: 'Ma Santé'),
          BottomNavigationBarItem(icon: Icon(Icons.shopping_cart), label: 'Panier (${panier.length})'),
        ],
      ),
    );
  }
}

// ==================== 1. ACCUEIL ====================
class AccueilPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('NUTRI-AFRICA'), centerTitle: true, backgroundColor: Colors.green),
      body: ListView(
        padding: EdgeInsets.all(16),
        children: [
          Container(
            padding: EdgeInsets.all(16),
            decoration: BoxDecoration(color: Colors.green[50], borderRadius: BorderRadius.circular(12)),
            child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
              Text('Mange sain, vis mieux', style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold)),
              Text('Partout en Afrique. Yaoundé - Douala - Dakar - Abidjan'),
              SizedBox(height: 10),
              ElevatedButton(onPressed: () {
                Navigator.push(context, MaterialPageRoute(builder: (_) => ConsultationPage()));
              }, child: Text('Consulter un Nutritionniste - 5 000 FCFA')),
            ]),
          ),
          SizedBox(height: 16),
          Text('NOS REPAS SAINS DU JOUR', style: TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
          SizedBox(height: 8),
          ...plats.map((p) => Card(
            child: ListTile(
              leading: CircleAvatar(child: Text('🍽️')),
              title: Text(p.nom),
              subtitle: Text('${p.description} - ${p.calories} kcal - ${p.categorie}'),
              trailing: Text('${p.prix} F', style: TextStyle(fontWeight: FontWeight.bold, color: Colors.green)),
              onTap: () {
                panier.add({'nom': p.nom, 'prix': p.prix, 'type': 'Repas'});
                ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text('${p.nom} ajouté au panier')));
              },
            ),
          )).toList(),
          SizedBox(height: 16),
          Text('MARCHÉ VIVRIER - PRODUITS FRAIS', style: TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
          SizedBox(height: 8),
          Container(
            padding: EdgeInsets.all(12),
            color: Colors.orange[50],
            child: Text('Achetez directement chez les agriculteurs du Cameroun. Livraison en 60 min. Manioc, Plantain, Légumes, Riz...'),
          ),
          ElevatedButton(onPressed: () {
            // Aller au marché
            ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text('Clique en bas sur Marché Vivrier')));
          }, child: Text('VOIR LE MARCHÉ VIVRIER')),
        ],
      ),
    );
  }
}

// ==================== 2. MARCHÉ VIVRIER - NOUVEAU ====================
class MarcheVivrierPage extends StatefulWidget {
  @override
  _MarcheVivrierPageState createState() => _MarcheVivrierPageState();
}

class _MarcheVivrierPageState extends State<MarcheVivrierPage> {
  String filtre = 'Tous';
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Marché Vivrier'), backgroundColor: Colors.orange),
      body: Column(
        children: [
          Padding(padding: EdgeInsets.all(12), child: TextField(decoration: InputDecoration(hintText: 'Rechercher manioc, plantain, riz...', prefixIcon: Icon(Icons.search), border: OutlineInputBorder(borderRadius: BorderRadius.circular(20))))),
          SingleChildScrollView(scrollDirection: Axis.horizontal, child: Row(children: ['Tous', 'Tubercules', 'Légumes', 'Céréales', 'Autres'].map((c) => Padding(padding: EdgeInsets.symmetric(horizontal: 4), child: ChoiceChip(label: Text(c), selected: filtre == c, onSelected: (_) => setState(() => filtre = c)))).toList())),
          Expanded(child: ListView.builder(itemCount: vivriers.length, itemBuilder: (context, i) {
            var v = vivriers[i];
            return Card(margin: EdgeInsets.symmetric(horizontal: 12, vertical: 6), child: ListTile(
              leading: Text(v.imageEmoji, style: TextStyle(fontSize: 30)),
              title: Text(v.nom, style: TextStyle(fontWeight: FontWeight.bold)),
              subtitle: Text('${v.unite} - Vendeur: ${v.vendeur}\nLivraison: Yaoundé, Douala, Bafoussam'),
              isThreeLine: true,
              trailing: Column(mainAxisAlignment: MainAxisAlignment.center, children: [
                Text('${v.prix} FCFA', style: TextStyle(fontWeight: FontWeight.bold, color: Colors.green)),
                SizedBox(height: 4),
                ElevatedButton(onPressed: () { setState(() { panier.add({'nom': v.nom, 'prix': v.prix, 'type': 'Vivrier'}); }); ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text('${v.nom} ajouté'))); }, child: Text('Acheter')),
              ]),
            ));
          })),
        ],
      ),
    );
  }
}

// ==================== 3. PROFIL SANTÉ + IMC ====================
class ProfilSantePage extends StatefulWidget {
  @override
  _ProfilSantePageState createState() => _ProfilSantePageState();
}

class _ProfilSantePageState extends State<ProfilSantePage> {
  TextEditingController poidsCtrl = TextEditingController(text: '70');
  TextEditingController tailleCtrl = TextEditingController(text: '1.65');
  double imc = 0;
  String resultat = '';
  String objectif = 'Perte de poids';

  void calculerIMC() {
    double poids = double.tryParse(poidsCtrl.text) ?? 0;
    double taille = double.tryParse(tailleCtrl.text) ?? 1;
    setState(() {
      imc = poids / (taille * taille);
      if (imc < 18.5) resultat = 'Sous-poids - Tu dois manger plus';
      else if (imc < 25) resultat = 'Poids normal - Bravo ! Continue';
      else if (imc < 30) resultat = 'Surpoids - Plan Minceur recommandé';
      else resultat = 'Obésité - Consulte un nutritionniste vite';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Ma Santé - IMC'), backgroundColor: Colors.green),
      body: Padding(padding: EdgeInsets.all(16), child: ListView(children: [
        Text('Ton Profil Santé', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
        SizedBox(height: 12),
        TextField(controller: poidsCtrl, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Poids en kg (ex: 70)', border: OutlineInputBorder())),
        SizedBox(height: 12),
        TextField(controller: tailleCtrl, keyboardType: TextInputType.number, decoration: InputDecoration(labelText: 'Taille en mètre (ex: 1.65)', border: OutlineInputBorder())),
        SizedBox(height: 12),
        DropdownButtonFormField<String>(value: objectif, items: ['Perte de poids', 'Diabétique', 'Sportif', 'Femme enceinte', 'Prise de masse'].map((e) => DropdownMenuItem(value: e, child: Text(e))).toList(), onChanged: (v) => setState(() => objectif = v!), decoration: InputDecoration(labelText: 'Ton objectif', border: OutlineInputBorder())),
        SizedBox(height: 16),
        ElevatedButton(onPressed: calculerIMC, child: Text('CALCULER MON IMC')),
        SizedBox(height: 16),
        if (imc > 0) Container(padding: EdgeInsets.all(16), decoration: BoxDecoration(color: Colors.green[50], borderRadius: BorderRadius.circular(12)), child: Column(children: [
          Text('Ton IMC : ${imc.toStringAsFixed(1)}', style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
          Text(resultat, textAlign: TextAlign.center),
          SizedBox(height: 8),
          Text('Objectif: $objectif'),
          SizedBox(height: 8),
          Text('Plan recommandé: 7 jours de repas sains + 1 consultation'),
        ])),
        SizedBox(height: 16),
        Text('Tes Nutritionnistes', style: TextStyle(fontWeight: FontWeight.bold)),
        ListTile(leading: CircleAvatar(child: Text('Dr')), title: Text('Dr. Ngo Bell - Nutritionniste'), subtitle: Text('Yaoundé - 10 ans d\'expérience - Diplôme vérifié'), trailing: Icon(Icons.verified, color: Colors.blue)),
        ListTile(leading: CircleAvatar(child: Text('Dr')), title: Text('Dr. Kamga - Diététicien'), subtitle: Text('Douala - Spécialiste diabète'), trailing: Icon(Icons.verified, color: Colors.blue)),
      ])),
    );
  }
}

// ==================== 4. CONSULTATION ====================
class ConsultationPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(appBar: AppBar(title: Text('Consultation')), body: Padding(padding: EdgeInsets.all(16), child: Column(children: [
      Text('Choisis ton nutritionniste', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
      Card(child: ListTile(title: Text('Dr. Ngo Bell'), subtitle: Text('Disponible aujourd\'hui à 15h - Vidéo 30 min'), trailing: Text('5 000 FCFA'))),
      Card(child: ListTile(title: Text('Dr. Kamga'), subtitle: Text('Disponible demain à 10h - Chat'), trailing: Text('5 000 FCFA'))),
      SizedBox(height: 20),
      TextField(decoration: InputDecoration(hintText: 'Décris ton problème : Je veux maigrir, je suis diabétique...', border: OutlineInputBorder()), maxLines: 3),
      SizedBox(height: 16),
      ElevatedButton(onPressed: () { ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text('Demande envoyée au nutritionniste. Réponse en moins de 24h.'))); }, child: Text('ENVOYER MA DEMANDE')),
    ])));
  }
}

// ==================== 5. PANIER + PAIEMENT ====================
class PanierPage extends StatefulWidget {
  @override
  _PanierPageState createState() => _PanierPageState();
}

class _PanierPageState extends State<PanierPage> {
  String paiement = 'MTN MoMo';
  int get total => panier.fold(0, (sum, item) => sum + (item['prix'] as int));
  @override
  Widget build(BuildContext context) {
    return Scaffold(appBar: AppBar(title: Text('Mon Panier'), backgroundColor: Colors.green), body: Column(children: [
      Expanded(child: panier.isEmpty ? Center(child: Text('Ton panier est vide.\nVa dans Accueil ou Marché Vivrier pour acheter.', textAlign: TextAlign.center)) : ListView.builder(itemCount: panier.length, itemBuilder: (context, i) {
        return ListTile(leading: Icon(panier[i]['type'] == 'Vivrier' ? Icons.store : Icons.restaurant), title: Text(panier[i]['nom']), subtitle: Text(panier[i]['type']), trailing: Text('${panier[i]['prix']} FCFA'));
      })),
      Container(padding: EdgeInsets.all(16), child: Column(children: [
        Row(mainAxisAlignment: MainAxisAlignment.spaceBetween, children: [Text('Total :', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)), Text('$total FCFA', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold, color: Colors.green))]),
        SizedBox(height: 10),
        DropdownButtonFormField<String>(value: paiement, items: ['MTN MoMo', 'Orange Money', 'Wave', 'Espèces à la livraison'].map((e) => DropdownMenuItem(value: e, child: Text(e))).toList(), onChanged: (v) => setState(() => paiement = v!), decoration: InputDecoration(labelText: 'Mode de paiement', border: OutlineInputBorder())),
        SizedBox(height: 12),
        SizedBox(width: double.infinity, child: ElevatedButton(onPressed: () {
          if (panier.isEmpty) return;
          showDialog(context: context, builder: (_) => AlertDialog(title: Text('Commande confirmée !'), content: Text('Merci ! Tu as payé $total FCFA avec $paiement.\nTon livreur arrive en 60 min à Yaoundé.\nTu peux suivre ta livraison en temps réel.'), actions: [TextButton(onPressed: () { setState(() { panier.clear(); }); Navigator.pop(context); }, child: Text('OK'))]));
        }, child: Text('PAYER $total FCFA - $paiement'))),
      ])),
    ]));
  }
}
