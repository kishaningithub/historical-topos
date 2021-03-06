+++
date = "2017-03-30"
title = "Parcourir la collection au complet"
+++

Pour trouver des cartes par nom de lieu, utilisez le champ de recherche ou parcourez la liste ci-dessous. Une fois que vous avez trouvé la carte, allez à Voir dans GeoPortal pour visualiser cette carte dans Scholars GeoPortal. Ceci vous permettra de superposer la carte sur une carte de base actuelle, vous permettant ainsi de découvrir les différents changements au fil du temps. Cela vous permettra également d’obtenir des renseignements plus détaillés sur la carte. 

Pour en savoir plus sur l’utilisation de l’index des cartes pour faire une recherche de cartes dans Scholars GeoPortal, veuillez aller à [Utiliser les cartes](../using-maps/).

<input placeholder="Recherche par nom de carte de feuille" name="Place name search" id="index-filter" type="text" aria-label="Recherche par nom de carte de feuille"/>

<script>
// Import a json file (previously sorted by place name, then year) and display, keeping all of the items with the same place name displayed together

  $.getJSON("../../combined_namesort.json", function(json) {

    // Create an array from the json file
    var jsontext = JSON.parse(JSON.stringify(json));
    var lines = '';

    for (var i = 0; i<jsontext.length; i++) {
      var title = jsontext[i].title.replace(/[^a-zA-Z0-9-_]/g, '');

      // if the title for the current item is not the same as the previous one, print the place name
      if (jsontext[ (i===0) ? (jsontext.length-1) : (i-1)].title !== jsontext[i].title) {
        lines += '<div>';
        lines += '<a class="toggle-mapsheets" href="" data-target="' + title + '-section">' + jsontext[i].title + '</a></div>';
      }

      lines += '<div class="' + title + '-section sheet-item">';
      lines += '<p>Year: ' + jsontext[i].year + ' | ';
      lines += '<a href="http://geo.scholarsportal.info/#r/details/_uri@=' + jsontext[i].fullname + '&_add:true" target="_blank"> Voir dans GeoPortal<i class="fa fa-external-link" aria-hidden="true"></i></a>| '; 
      lines += '<a href="http://ocul.on.ca/topomaps/map-images/' + jsontext[i].fullname + '.jpg"> Télécharger l\'image</a></p>';
      lines += '</div>';

      // append the content into the div with the same id
      $(lines).appendTo('#index');

      // reset the lines variable so it isn't duplicated on the next loop
      lines = "";
    }

    // expand to see all sheets when the place name is clicked
    $( '.toggle-mapsheets' ).click(function(e) {
      e.preventDefault();
      var cssClass = $(e.target).data('target');
      $( '.' + cssClass ).toggle();
    });

    // Filter box
    $('#index-filter').keyup(function(){
        var valThis = $(this).val().toLowerCase();
        $('.sheet-item:visible').hide();

    if(valThis == ""){
        $('.toggle-mapsheets').show();           
    }

    else {
      $('.toggle-mapsheets').each(function(){
          var text = $(this).text().toLowerCase();
          (text.indexOf(valThis) >= 0) ? $(this).show() : $(this).hide();
      });
    };
  });
});
</script>

<div id="index"></div>
