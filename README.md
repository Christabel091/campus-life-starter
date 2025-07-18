.route-filter {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 16px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.08);
  margin: 20px auto;
  max-width: 700px;
  transition: all 0.3s ease;
}

.route-filter .filter-button {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.route-filter button {
  background-color: #f7f9fc;
  border: 1px solid #dcdfe6;
  border-radius: 10px;
  padding: 10px 16px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: background 0.2s ease;
}

.route-filter button:hover {
  background-color: #e6f0ff;
}

.route-filter button:active {
  background-color: #cce0ff;
}

.walk i {
  margin-right: 6px;
}

.route-filter .site-select {
  margin-top: 12px;
  font-size: 15px;
  font-weight: 500;
}

.rank-options,
.rank-options-sort {
  margin-top: 10px;
  padding: 10px 14px;
  border: 1px dashed #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
}

.rank-options p,
.rank-options-sort p {
  margin: 6px 0;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 6px;
  transition: background 0.2s ease;
}

.rank-options p:hover,
.rank-options-sort p:hover {
  background-color: #edf4ff;
}

.route-filter .error {
  color: #cc0000;
  margin-top: 10px;
  font-size: 14px;
}

.ranked-spot-item {
  margin: 10px auto;
  background-color: #ffffff;
  border: 1px solid #e1e4e8;
  padding: 15px;
  border-radius: 12px;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.06);
  cursor: pointer;
  transition: transform 0.2s ease, background-color 0.2s ease;
  max-width: 650px;
}

.ranked-spot-item:hover {
  background-color: #f4f9ff;
  transform: translateY(-2px);
}

.ranked-spot-item p {
  margin: 4px 0;
  font-size: 14px;
}



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




.walk.active-toggle {
  background-color: #007bff;
  color: #fff;
  border-color: #007bff;
}

.walk.active-toggle:hover {
  background-color: #0056b3;
}
