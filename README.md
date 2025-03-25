import React, { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

const cardsData = [
  { id: 1, term: "RGPD", definition: "Règlement Général sur la Protection des Données" },
  { id: 2, term: "CRM", definition: "Logiciel de gestion de la relation client" },
  { id: 3, term: "ERP", definition: "Système de gestion intégré pour l'entreprise" },
  { id: 4, term: "CMS", definition: "Système de gestion de contenu pour les sites web" },
];

export default function MemoryGame() {
  const [shuffledCards, setShuffledCards] = useState([]);
  const [selectedCards, setSelectedCards] = useState([]);
  const [matchedCards, setMatchedCards] = useState([]);

  useEffect(() => {
    const duplicatedCards = [...cardsData, ...cardsData]
      .sort(() => Math.random() - 0.5)
      .map((card, index) => ({ ...card, uniqueId: index }));
    setShuffledCards(duplicatedCards);
  }, []);

  const handleCardClick = (card) => {
    if (selectedCards.length < 2 && !selectedCards.includes(card.uniqueId)) {
      setSelectedCards([...selectedCards, card.uniqueId]);
    }
  };

  useEffect(() => {
    if (selectedCards.length === 2) {
      const [first, second] = selectedCards;
      const firstCard = shuffledCards.find((card) => card.uniqueId === first);
      const secondCard = shuffledCards.find((card) => card.uniqueId === second);
      if (firstCard.id === secondCard.id) {
        setMatchedCards([...matchedCards, firstCard.id]);
      }
      setTimeout(() => setSelectedCards([]), 1000);
    }
  }, [selectedCards, shuffledCards, matchedCards]);

  return (
    <div className="flex flex-col items-center p-4">
      <h2 className="text-lg font-bold mb-4">Jeu de mémoire : Associez les cartes</h2>
      <div className="grid grid-cols-4 gap-4">
        {shuffledCards.map((card) => (
          <Card
            key={card.uniqueId}
            className={`p-4 text-center cursor-pointer ${
              selectedCards.includes(card.uniqueId) || matchedCards.includes(card.id) ? "bg-blue-300" : "bg-gray-200"
            }`}
            onClick={() => handleCardClick(card)}
          >
            <CardContent>
              {selectedCards.includes(card.uniqueId) || matchedCards.includes(card.id) ? (
                <p>{card.term || card.definition}</p>
              ) : (
                <p>?</p>
              )}
            </CardContent>
          </Card>
        ))}
      </div>
      {matchedCards.length === cardsData.length && (
        <h2 className="mt-4 text-xl font-bold text-green-600">Bravo ! Vous avez tout trouvé !</h2>
      )}
    </div>
  );
}
