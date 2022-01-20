# referensi-web-service-java







package gov.dukcapil.web.apps.servicesDki.referensi;

import io.vertx.core.MultiMap;
import io.vertx.core.Vertx;
import io.vertx.core.http.HttpMethod;
import io.vertx.core.json.JsonArray;
import io.vertx.core.json.JsonObject;
import io.vertx.ext.web.Router;
import io.vertx.ext.web.RoutingContext;

import gov.dukcapil.web.apps.servicesDki.SubVerticalServiceDki;
import gov.dukcapil.web.libs.helper.Dukcapil;

public class Wilayah extends SubVerticalServiceDki {
	private static final String PKG_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_SETUP_PROP.PROC_VIEW(?,?,?,?)";
	private static final String PKG_KAB_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_SETUP_KAB.PROC_VIEW(?,?,?,?,?)";
	private static final String PKG_KEC_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_SETUP_KEC.PROC_VIEW(?,?,?,?,?,?)";
	private static final String PKG_KEL_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_SETUP_KEL.PROC_VIEW(?,?,?,?,?,?,?)";
	private static final String PKG_RW_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_RW.PROC_VIEW(?,?,?,?,?,?,?,?)";
	private static final String PKG_RT_PROC_VIEW = "CALL SIAK_REFF.PKG_DKI_REF_RT.PROC_VIEW(?,?,?,?,?,?,?,?,?)";
	
	
	
	public Wilayah(Vertx vertx) {
		super(vertx);
	}

	@Override
	protected void getRouter(Router router) {
		router.route(HttpMethod.GET, "/getProp").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getProp(context);
		});
		
		router.route(HttpMethod.GET, "/getKab").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getKab(context);
		});
		
		router.route(HttpMethod.GET, "/getKec").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getKec(context);
		});
		
		router.route(HttpMethod.GET, "/getKel").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getKel(context);
		});
		
		router.route(HttpMethod.GET, "/getRw").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getRw(context);
		});
		
		router.route(HttpMethod.GET, "/getRt").handler(context -> {
			context.put(Dukcapil.REQ_STARTTIME, System.currentTimeMillis());
			getRt(context);
		});
		
	}
	
	private void getProp(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!paramQuery.contains("noProp")) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
			return;
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);

		dbService.callQuery(PKG_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(1);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(2));
					responseClientError(context, status, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(3));
					responseClientSuccess(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}

		});
	}
	
	private void getKab(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!(paramQuery.contains("noKab") && paramQuery.contains("noProp"))) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
			return;
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		paramIn.add(Long.parseLong(paramQuery.get("noKab")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);

		dbService.callQuery(PKG_KAB_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(2);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(3));
					responseClientError(context, status, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(4));
					responseClientSuccess(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}

		});
	}
	
	private void getKec(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!(paramQuery.contains("noProp") &&
			paramQuery.contains("noKab") &&
			paramQuery.contains("noKec"))) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		paramIn.add(Long.parseLong(paramQuery.get("noKab")));
		paramIn.add(Long.parseLong(paramQuery.get("noKec")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);
		
		dbService.callQuery(PKG_KEC_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(3);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(4));
					responseClientError(context, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(5));
					responseClientError(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}
		});
	}
	
	private void getKel(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!(paramQuery.contains("noProp") &&
			paramQuery.contains("noKab") &&
			paramQuery.contains("noKec") &&
			paramQuery.contains("noKel"))) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		paramIn.add(Long.parseLong(paramQuery.get("noKab")));
		paramIn.add(Long.parseLong(paramQuery.get("noKec")));
		paramIn.add(Long.parseLong(paramQuery.get("noKel")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);
		
		dbService.callQuery(PKG_KEL_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(4);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(5));
					responseClientError(context, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(6));
					responseClientError(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}
		});
	}
	
	private void getRw(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!(paramQuery.contains("noProp") &&
			paramQuery.contains("noKab") &&
			paramQuery.contains("noKec") &&
			paramQuery.contains("noKel") &&
			paramQuery.contains("noRw"))) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		paramIn.add(Long.parseLong(paramQuery.get("noKab")));
		paramIn.add(Long.parseLong(paramQuery.get("noKec")));
		paramIn.add(Long.parseLong(paramQuery.get("noKel")));
		paramIn.add(Long.parseLong(paramQuery.get("noRw")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);
		
		dbService.callQuery(PKG_RW_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(5);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(6));
					responseClientError(context, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(7));
					responseClientError(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}
		});
	}
	
	private void getRt(RoutingContext context) {
		final MultiMap paramQuery = context.queryParams();
		if (!(paramQuery.contains("noProp") &&
			paramQuery.contains("noKab") &&
			paramQuery.contains("noKec") &&
			paramQuery.contains("noKel") &&
			paramQuery.contains("noRw") &&
			paramQuery.contains("noRt"))) {
			responseBody = new JsonObject();
			responseBody.put(Dukcapil.RESP_MESSAGE, "Invalid Parameter");
			responseClientError(context, responseBody);
		}
		
		final JsonArray paramIn = new JsonArray();
		paramIn.add(Long.parseLong(paramQuery.get("noProp")));
		paramIn.add(Long.parseLong(paramQuery.get("noKab")));
		paramIn.add(Long.parseLong(paramQuery.get("noKec")));
		paramIn.add(Long.parseLong(paramQuery.get("noKel")));
		paramIn.add(Long.parseLong(paramQuery.get("noRw")));
		paramIn.add(Long.parseLong(paramQuery.get("noRt")));
		
		final JsonArray paramOut = new JsonArray();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.addNull();
		paramOut.add(java.sql.Types.SMALLINT);
		paramOut.add(java.sql.Types.VARCHAR);
		paramOut.add(java.sql.Types.VARCHAR);
		
		dbService.callQuery(PKG_RT_PROC_VIEW, paramIn, paramOut, res -> {
			if (res.succeeded()) {
				final JsonArray resultDb = res.result();
				final int status = resultDb.getInteger(6);
				if (status != Dukcapil.RESP200) {
					responseBody = new JsonObject();
					responseBody.put(Dukcapil.RESP_MESSAGE, resultDb.getString(7));
					responseClientError(context, responseBody);
				} else {
					responseBody = new JsonObject(resultDb.getString(8));
					responseClientError(context, responseBody);
				}
			} else {
				responseBody = new JsonObject();
				responseBody.put(Dukcapil.RESP_MESSAGE, res.cause().getMessage());
				responseClientError(context, responseBody);
			}
		});
	}
	
	
	
	

}
