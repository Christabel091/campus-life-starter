/**
 * Dynamic Custom Pathfinding Algorithm
 * - Returns multiple paths to different goal nodes
 * - Enables dynamic ranking by cost, distance, reliability, etc.
 * - Builds on the existing Node and buildGraph structure
 */

import { getTimePenalty, getDistance, calculatePriority, smartHeuristic } from "./heuristics";

export function dynamicPathFinder(startNode, goalNodes, options = {}) {
  const queue = [{ node: startNode, cost: 0 }];
  const visited = new Set();
  const cameFrom = {};
  const costSoFar = { [startNode.id]: 0 };
  const hour = new Date().getHours();
  const foundPaths = [];

  while (queue.length > 0) {
    queue.sort((a, b) => a.cost - b.cost);
    const { node: current } = queue.shift();

    if (visited.has(current.id)) continue;
    visited.add(current.id);

    const matchingGoal = goalNodes.find((goal) => goal.id === current.id);
    if (matchingGoal) {
      let path = [current];
      let totalCost = costSoFar[current.id];
      while (cameFrom[path[0].id]) {
        path.unshift(cameFrom[path[0].id]);
      }
      foundPaths.push({ path, goal: current, totalCost });
      continue; // Continue exploring other paths
    }

    for (const edge of current.edges) {
      const neighbor = edge.node;
      const newCost = costSoFar[current.id] + edge.weight;
      const unreliability = 0.3; // Placeholder — could use actual value
      const heuristic = smartHeuristic(
        neighbor,
        goalNodes[0], // heuristic is relative; all goals assumed close
        unreliability,
        getTimePenalty(hour)
      );
      const priority = calculatePriority(newCost, heuristic, 1.1);

      if (!(neighbor.id in costSoFar) || newCost < costSoFar[neighbor.id]) {
        costSoFar[neighbor.id] = newCost;
        cameFrom[neighbor.id] = current;
        queue.push({ node: neighbor, cost: priority });
      }
    }
  }

  return foundPaths;
}
