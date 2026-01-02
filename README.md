# Dynamic-Multi-Target-Priority-Selector
# Priorit# PrioritySelector.py (Updated Implementation)

class PrioritySelector:
    def __init__(self):
        self.DISTANCE_WEIGHT = 0.3
        self.HP_WEIGHT = 0.3
        # NEW: High weight for preventing rivals from staying in top ranks
        self.RANK_WEIGHT = 0.4 

    def calculate_threat_score(self, distance: float, hp: float, rank: int) -> float:
        """
        NEW: Enhanced formula including Rank Priority.
        """
        distance_factor = 1.0 / (distance + 1.0)
        hp_factor = 1.0 / (hp + 1.0)
        
        # Rank factor: If rank is 1 or 2, boost priority significantly
        rank_factor = 1.0 if rank <= 2 else 0.2
        
        return (distance_factor * self.DISTANCE_WEIGHT) + \
               (hp_factor * self.HP_WEIGHT) + \
               (rank_factor * self.RANK_WEIGHT)

    def get_best_target(self, enemies: list[dict]) -> dict:
        best_target = None
        max_score = -1.0

        for enemy in enemies:
            # Now passing the enemy's current rank into the score
            score = self.calculate_threat_score(enemy['distance'], enemy['hp'], enemy['rank'])
            enemy['current_threat_score'] = score # For debugging
            
            if score > max_score:
                max_score = score
                best_target = enemy
        
        return best_target

# Example Usage
if __name__ == "__main__":
    selector = PrioritySelector()
    enemy_list = [
        {"id": "Weak_Mob", "distance": 5, "hp": 20, "rank": 10},  # Close and weak, but low rank
        {"id": "Top_Rival", "distance": 25, "hp": 80, "rank": 1}  # Stronger and further, but IS RANK 1
    ]
    
    target = selector.get_best_target(enemy_list)
    print(f"Targeting: {target['id']} (Score: {target['current_threat_score']:.2f})")
    print("Reason: Priority shifted to neutralize the top-ranking competitor.")
