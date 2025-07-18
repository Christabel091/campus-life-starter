<div className="route-filter">
          {<DestSearch onSelect={(loc) => setDestinationLocation(loc)} />}
          <div className="filter-button">
            <button className="fab-rotate">Rotate Map</button>

            <button
              className={`walk ${isDriving ? "active-toggle" : ""}`}
              onClick={handleToggle}
            >
              <i className="fas fa-car"></i>
              <p>Driving</p>
            </button>

            <button
              className={`walk ${!isDriving ? "active-toggle" : ""}`}
              onClick={handleToggle}
            >
              <i className="fas fa-walking"></i>
              <p>Walking</p>
            </button>
            <label>
              <input
                type="checkbox"
                checked={hideViewed}
                className="ranked-spot-checkbox"
                onChange={(e) => {
                  setHideViewed(!hideViewed);
                }}
              />
              Hide previously shown spots
            </label>
            <div className="site-select">
              <div
                onClick={() => {
                  setRankBy(!rankBy);
                  setSortBy(null);
                }}
              >
                <p>Rank spots by:{rankBy ? "▼" : "▶"}</p>
              </div>
            </div>
          </div>
          {rankBy && (
            <div className="rank-options">
              <p
                onClick={() => {
                  setSortBy("distance");
                  setRankValue("closestToUser");
                }}
              >
                closest to you
              </p>
              <p
                value="closestToDestination"
                onClick={() => {
                  setSortBy("distance");
                  setRankValue("closestToDestination");
                }}
              >
                Closest to destination
              </p>
              <p
                value="cheapest"
                onClick={() => {
                  setSortBy("price");
                  setRankValue("cheapest");
                }}
              >
                All Cheapest
              </p>
            </div>
          )}
          {sortBy && sortBy === "distance" && (
            <div className="rank-options-sort">
              <p>Sort By: </p>
              <p
                onClick={() => {
                  setSortByValue("price");
                }}
              >
                Price
              </p>
              <p
                onClick={() => {
                  setSortByValue("None");
                }}
              >
                None
              </p>
            </div>
          )}

          {sortBy && sortBy === "price" && (
            <div className="rank-options-sort">
              <p
                onClick={() => {
                  setSortByValue("DistanceToUser");
                }}
              >
                closest to you
              </p>

              <p
                onClick={() => {
                  setSortByValue("DistanceToDest");
                }}
              >
                closest to your destination
              </p>
              <p
                onClick={() => {
                  setSortByValue("None");
                }}
              >
                None
              </p>
            </div>
          )}
          <button onClick={handleRankTypeChange}>
            <p>Rank</p>
          </button>
          <p>{error}</p>
        </div>
        {!loaded && (
          <div className="skeleton-wrapper">
            {Array.from({ length: 5 }).map((_, index) => (
              <div key={index} className="skeleton-spot-card" />
            ))}
          </div>
        )}
        {showRouteList &&
          loaded &&
          rankPathChoosen?.slice(0, 5).map((path, index) => (
            <div
              key={index}
              className="ranked-spot-item"
              onClick={() => showRoute(path)}
            >
              <p>
                Lot: {spots.find((spot) => spot.id === path.goal.id)?.lotName}
              </p>
              <p>Distance: {Math.round(path.totalDistance)}m</p>
              <p>ETA: {Math.round(path.goal.drivingMinutesFromUser)} min</p>
              <p>Price: ${path.totalPrice}</p>
            </div>
          ))}
